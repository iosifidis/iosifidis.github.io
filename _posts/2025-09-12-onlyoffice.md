---
layout: post
title: "Ενεργοποίηση επεξεργασίας ONLYOFFICE σε κινητά & αύξηση ορίου συνδέσεων"
date: 2025-09-12 12:00:00
description: Ο πλήρης οδηγός για να ενεργοποιήσετε την επεξεργασία σε κινητά στο ONLYOFFICE και να αφαιρέσετε το όριο των 20 συνδέσεων. Λύσεις με Docker και JS.
tags:
  - onlyoffice
  - nextcloud
  - docker
  - js
categories:
  - Greek
  - ONLYOFFICE
  - NEXTCLOUD
twitter_text: "Ενεργοποίηση επεξεργασίας ONLYOFFICE σε κινητά & αύξηση ορίου συνδέσεων"
---

![Ενεργοποίηση επεξεργασίας ONLYOFFICE σε κινητά & αύξηση ορίου συνδέσεων](/post_images/onlyoffice/onlyoffice-connections.png "Ενεργοποίηση επεξεργασίας ONLYOFFICE σε κινητά & αύξηση ορίου συνδέσεων"){:width="320px"}

# Τροποποίηση του ONLYOFFICE για επεξεργασία σε κινητά & απεριόριστες συνδέσεις

Αυτός ο οδηγός αποτελεί μια σύνοψη μιας μακράς και λεπτομερούς συζήτησης της κοινότητας, η οποία ξεκίνησε με σκοπό την επαναφορά της δυνατότητας επεξεργασίας εγγράφων σε κινητές συσκευές (Mobile Edit) και την αύξηση του ορίου των ταυτόχρονων συνδέσεων στην δωρεάν έκδοση του [ONLYOFFICE Document Server](https://www.onlyoffice.com/?rel=iosifidis.gr). Οι λύσεις εξελίχθηκαν με την πάροδο του χρόνου, καθώς νέες εκδόσεις του ONLYOFFICE κυκλοφορούσαν.

---

## Μέθοδος 1: Η Αρχική Προσέγγιση του 'Nemskiller'
Η αρχική λύση βασίστηκε σε μια προ-μεταγλωττισμένη εικόνα Docker που αφαιρούσε τους περιορισμούς. Αυτή η μέθοδος είναι πλέον παλιά, αλλά παρατίθεται για ιστορικούς λόγους.

### Βήμα 1: Λήψη της ειδικής εικόνας Docker
Η κοινότητα δημιούργησε μια ειδική εικόνα Docker με τις απαραίτητες τροποποιήσεις. Η λήψη γινόταν με την παρακάτω εντολή:
{% highlight ruby %}
docker pull nemskiller007/officeunleashed
{% endhighlight %}

### Βήμα 2: Εκκίνηση του Container
Η εκτέλεση του container γινόταν με την ακόλουθη εντολή, η οποία χαρτογραφούσε τη θύρα 80 του container στην 8000 του host:
{% highlight ruby %}
docker run -i -t -d -p 8000:80 --restart=always nemskiller007/officeunleashed
{% endhighlight %}

### Βήμα 3: (Προαιρετικά) Ορισμός κωδικού πρόσβασης (JWT)
Για την προστασία της εγκατάστασης με κωδικό, έπρεπε να τροποποιηθεί το αρχείο ρυθμίσεων μέσα στο container.

1.  Βρείτε το CONTAINER_ID με την εντολή:
{% highlight ruby %}
docker ps -a
{% endhighlight %}
2.  Μπείτε στο shell του container:
{% highlight ruby %}
docker exec -it CONTAINER_ID /bin/bash
{% endhighlight %}
3.  Επεξεργαστείτε το αρχείο ρυθμίσεων:
{% highlight ruby %}
nano /out/linux_64/onlyoffice/documentserver/server/Common/config/default.json
{% endhighlight %}
4.  Πηγαίνετε στη γραμμή 155 και αντικαταστήστε τις τιμές "secret" με τον δικό σας κωδικό.
5.  Στις γραμμές 163 έως 170, αλλάξτε τις τιμές από `false` σε `true` για το "request", "inbox" και "outbox". Παράδειγμα:
{% highlight ruby %}
"request": {
 "inbox": true,
 "outbox": true
}
{% endhighlight %}
6.  Αποθηκεύστε τις αλλαγές και κάντε επανεκκίνηση του container:
{% highlight ruby %}
docker stop CONTAINER_ID
docker start CONTAINER_ID
{% endhighlight %}

### Βήμα 4: Ρύθμιση Reverse Proxy (Nginx)
Για την πρόσβαση μέσω ενός FQDN (π.χ., `office.mydomain.com`) και τη χρήση SSL, απαιτείται ένας reverse proxy. Ακολουθεί ένα παράδειγμα αρχείου ρυθμίσεων για το Nginx:
{% highlight ruby %}
upstream docservice {
  server office.MYDOMAIN.COM:8000;
}

map $http_host $this_host {
  "" $host;
  default $http_host;
}

map $http_x_forwarded_proto $the_scheme {
  default $http_x_forwarded_proto;
  "" $scheme;
}

map $http_x_forwarded_host $the_host {
  default $http_x_forwarded_host;
  "" $this_host;
}

map $http_upgrade $proxy_connection {
  default upgrade;
  "" close;
}

proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $proxy_connection;
proxy_set_header X-Forwarded-Host $the_host;
proxy_set_header X-Forwarded-Proto $the_scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

server {
  server_tokens off;
  listen 0.0.0.0:80;
  listen [::]:80;
  server_name office.MYDOMAIN.COM;

  location / {
    proxy_pass http://docservice;
    proxy_http_version 1.1;
  }
}
{% endhighlight %}
Μετά τη ρύθμιση, θα πρέπει να κάνετε reload τον Nginx και να εγκαταστήσετε ένα πιστοποιητικό SSL (π.χ., με το Letsencrypt). Εάν το [Nextcloud](https://nextcloud.com/?rel=iosifidis.gr) και το ONLYOFFICE Docker τρέχουν στον ίδιο host, ίσως χρειαστεί να προσθέσετε την γραμμή `127.0.0.1 office.MYDOMAIN.COM` στο αρχείο `/etc/hosts`.

---

## Μέθοδος 2: Η νεότερη & απλούστερη λύση του 'Mageunic' (Για OnlyOffice v7.0+)
Αυτή η μέθοδος αναδείχθηκε ως η πιο δημοφιλής και απλή για νεότερες εκδόσεις του ONLYOFFICE (7.0 και μετά). Δεν απαιτεί ειδική εικόνα Docker, αλλά την τροποποίηση αρχείων JavaScript στην επίσημη εγκατάσταση.

### Η διαδικασία
Η λογική είναι να βρεθεί μια συνάρτηση που ελέγχει αν η επεξεργασία υποστηρίζεται και να την αλλάξετε ώστε να επιστρέφει πάντα "true".

1.  Εγκαταστήστε την επίσημη έκδοση του ONLYOFFICE Document Server (είτε μέσω Docker είτε ως πακέτο).
2.  Εντοπίστε τα παρακάτω 3 αρχεία JavaScript στον server σας. Η διαδρομή μπορεί να διαφέρει, αλλά συνήθως βρίσκεται μέσα στο `/var/www/onlyoffice/documentserver/web-apps/apps/`.
    *   `.../spreadsheeteditor/mobile/dist/js/app.js`
    *   `.../documenteditor/mobile/dist/js/app.js`
    *   `.../presentationeditor/mobile/dist/js/app.js`
3.  Ανοίξτε κάθε ένα από αυτά τα αρχεία με έναν επεξεργαστή κειμένου (π.χ., `sudo nano ...`).
4.  Κάντε αναζήτηση για την παρακάτω συμβολοσειρά:
{% highlight ruby %}
isSupportEditFeature=function(){return!1}
{% endhighlight %}
5.  Αντικαταστήστε την με:
{% highlight ruby %}
isSupportEditFeature=function(){return 1}
{% endhighlight %}
6.  Αποθηκεύστε τα αρχεία και κάντε επανεκκίνηση τον Nginx. Είναι επίσης καλή ιδέα να καθαρίσετε την cache του browser σας.

### Αυτοματοποίηση με Shell Script
Ο χρήστης 'jorsn' πρότεινε ένα script για την αυτοματοποίηση αυτής της διαδικασίας σε ένα Docker container:
{% highlight ruby %}
for f in $ROOT/var/www/onlyoffice/documentserver/web-apps/apps/*/mobile/dist/js/app.js; do
  sed -i 's/\(isSupportEditFeature:function(){return\)!1/\11/' "$f"
  && { cat "$f" | gzip > "$f".gz; }
done
{% endhighlight %}
Εδώ, το `$ROOT` αντιπροσωπεύει τη ριζική διαδρομή του filesystem μέσα στο container. Αυτή η μέθοδος τροποποιεί τα αρχεία και τα συμπιέζει ξανά σε gzip, όπως αναμένεται από τον server.

---

## Μέθοδος 3: Χρήση ενεργών Builds της κοινότητας (προτεινόμενη λύση)
Μετά την αρχική προσπάθεια, άλλοι χρήστες όπως ο **'thomisus'** ανέλαβαν τη συντήρηση ενημερωμένων Docker images και πακέτων .deb που περιλαμβάνουν αυτές τις τροποποιήσεις "out-of-the-box".

Η χρήση αυτών των builds είναι η ευκολότερη και πιο αξιόπιστη μέθοδος, καθώς η κοινότητα φροντίζει για τη συμβατότητα με τις τελευταίες εκδόσεις.

### Πού θα τα βρείτε:
*   **Docker Hub (thomisus):** [https://hub.docker.com/r/thomisus/onlyoffice-documentserver-unlimited](https://hub.docker.com/r/thomisus/onlyoffice-documentserver-unlimited)
*   **GitHub Container Registry (ghcr.io):**
{% highlight ruby %}
docker pull ghcr.io/thomisus/onlyoffice-documentserver-unlimited:latest
{% endhighlight %}
*   **GitHub Repository (για releases & πηγαίο κώδικα):** [https://github.com/thomisus/server/releases](https://github.com/thomisus/server/releases)

Η εγκατάσταση είναι τόσο απλή όσο το να τραβήξετε την εικόνα και να την εκτελέσετε, ακολουθώντας παρόμοιες οδηγίες με τη Μέθοδο 1 για τη ρύθμιση του reverse proxy.

---

## Συχνά προβλήματα & Τελευταίες εξελίξεις
Η συζήτηση στο φόρουμ κάλυψε πολλά ζητήματα. Εδώ είναι μερικά από τα πιο σημαντικά:

*   **JWT (JSON Web Token):** Η ρύθμιση του JWT γίνεται χειροκίνητα στο `default.json` ή στο `local.json`. Είναι ο προτιμώμενος τρόπος για να ασφαλίσετε το Document Server σας, ειδικά όταν συνδέεται με το Nextcloud/ownCloud.
*   **Build από τον πηγαίο κώδικα:** Αρκετοί χρήστες προσπάθησαν να μεταγλωττίσουν τον πηγαίο κώδικα μόνοι τους χρησιμοποιώντας τα `build_tools` του ONLYOFFICE. Η διαδικασία αποδείχθηκε **εξαιρετικά περίπλοκη** και απαιτούσε παλαιότερες εκδόσεις του Ubuntu (π.χ., 14.04) για να λειτουργήσει σωστά.
*   **Σφάλματα 502 Bad Gateway:** Συνήθως υποδεικνύουν λανθασμένη ρύθμιση του reverse proxy (Nginx/Apache) ή ότι το εσωτερικό service του ONLYOFFICE δεν έχει ξεκινήσει σωστά.
*   **Προβλήματα με SSL:** Το ONLYOFFICE σε περιβάλλον παραγωγής πρέπει πάντα να βρίσκεται πίσω από έναν reverse proxy που διαχειρίζεται το SSL. Η χρήση self-signed certificates απευθείας στο container μπορεί να προκαλέσει προβλήματα, ειδικά με τις mobile εφαρμογές.
*   **Η εξέλιξη της λειτουργίας:** Σε κάποιες μεταγενέστερες επίσημες εκδόσεις (π.χ., 6.1.1, 6.4.1), η επεξεργασία σε κινητά φάνηκε να επιστρέφει προσωρινά, για να αφαιρεθεί ξανά αργότερα, προκαλώντας σύγχυση. Αυτό ενισχύει την ανάγκη για τις λύσεις της κοινότητας.
*   **Ενημερώσεις & Ασυμβατότητες:** Κάθε νέα έκδοση του Nextcloud ή του ONLYOFFICE μπορεί να απαιτήσει αλλαγές. Για παράδειγμα, νεότερες εκδόσεις του Nextcloud Connector απαιτούν νεότερες εκδόσεις του ONLYOFFICE Document Server.

---

### Συμπέρασμα
Για χρήστες που θέλουν να ενεργοποιήσουν την επεξεργασία σε κινητά και να αφαιρέσουν το όριο συνδέσεων, η πιο **απλή και προτεινόμενη μέθοδος σήμερα** είναι η χρήση μιας έτοιμης εικόνας Docker από την κοινότητα, όπως αυτή που συντηρεί ο χρήστης **thomisus**.

Για πιο προχωρημένους χρήστες που χρησιμοποιούν την επίσημη έκδοση, η χειροκίνητη τροποποίηση των αρχείων JavaScript (Μέθοδος 2) παραμένει μια βιώσιμη λύση. Η προσπάθεια build από τον πηγαίο κώδικα δεν συνιστάται λόγω της πολυπλοκότητάς της.

Αυτός ο οδηγός αποτελεί απόδειξη της δύναμης της κοινότητας ανοιχτού κώδικα για την εξεύρεση λύσεων σε περιορισμούς λογισμικού. Ευχαριστούμε όλους τους συμμετέχοντες στη συζήτηση, ιδιαίτερα τους **Nemskiller**, **Mageunic**, **thomisus**, **jorsn**, και πολλούς άλλους που συνεισέφεραν.

Πηγή:
[OnlyOffice compiled with Mobile Edit Back](https://help.nextcloud.com/t/onlyoffice-compiled-with-mobile-edit-back/79282/321)

---

### Σημαντική αποποίηση ευθύνης
Το παρόν άρθρο αποτελεί μια σύνοψη τεχνικών λύσεων που προτάθηκαν από την κοινότητα χρηστών και δεν αποτελεί επίσημη τεκμηρίωση. Οι οδηγίες παρέχονται για εκπαιδευτικούς σκοπούς χωρίς καμία εγγύηση για την ορθότητα, την ασφάλεια ή την αποτελεσματικότητά τους

Η εφαρμογή των βημάτων που περιγράφονται γίνεται με δική σας αποκλειστικά ευθύνη. Ο συγγραφέας δεν φέρει καμία ευθύνη για οποιαδήποτε πιθανή ζημιά στο σύστημά σας, απώλεια δεδομένων ή κενά ασφαλείας που ενδέχεται να προκύψουν. Πριν από οποιαδήποτε επέμβαση, βεβαιωθείτε ότι έχετε δημιουργήσει πλήρη και λειτουργικά αντίγραφα ασφαλείας (backups).

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2025/09/onlyoffice.html](https://eiosifidis.blogspot.com/2025/09/onlyoffice.html){:target="\_blank"}
