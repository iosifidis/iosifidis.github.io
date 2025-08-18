---
layout: post
title: "Εγκατάσταση και ασφάλεια του Nextcloud server με το CrowdSec"
date: 2025-02-10 12:00:00
description: Οδηγός για ασφαλή εγκατάσταση Nextcloud με CrowdSec. Προστατεύστε τον server σας από επιθέσεις με αναλυτικά βήματα και όλες τις εντολές για Docker.
tags:
  - nextcloud
  - crowdsec
  - docker
  - security
categories:
  - Greek
  - NEXTCLOUD
  - CROWDSEC
twitter_text: "Εγκατάσταση και ασφάλεια του Nextcloud server με το CrowdSec"
---

![Εγκατάσταση και ασφάλεια του Nextcloud server με το CrowdSec](/post_images/nexcrowdsect/crowdsec-nextcloud.png "Εγκατάσταση και ασφάλεια του Nextcloud server με το CrowdSec"){:width="320px"}

# Οδηγός εγκατάστασης: Ασφαλίστε τον Nextcloud Server σας με το CrowdSec

## Τι είναι το CrowdSec και τι πετυχαίνει;

Το [CrowdSec](https://www.crowdsec.net/) είναι ένα σύγχρονο, δωρεάν και ανοιχτού κώδικα Σύστημα Πρόληψης Εισβολών (Intrusion Prevention System - IPS). Η λειτουργία του βασίζεται στη **συνεργατική ασφάλεια**. Λειτουργεί ως μια τεράστια, συλλογική "περιπολία γειτονιάς" για το διαδίκτυο, προστατεύοντας κάθε μέλος της κοινότητας από απειλές που ανιχνεύονται οπουδήποτε στον κόσμο.

- **Ανιχνεύει:** Το CrowdSec "διαβάζει" τα αρχεία καταγραφής (logs) των εφαρμογών σας (π.χ. του web server, του SSH, του Nextcloud) και αναγνωρίζει κακόβουλη συμπεριφορά με βάση συγκεκριμένα μοτίβα.
- **Αποκρίνεται:** Μόλις ανιχνεύσει έναν εισβολέα, μπλοκάρει την IP διεύθυνσή του σε επίπεδο firewall ή reverse proxy μέσω ενός ειδικού στοιχείου που ονομάζεται "bouncer".
- **Μοιράζεται:** Η IP του επιτιθέμενου αναφέρεται ανώνυμα σε ένα κεντρικό σύστημα. Αυτή η πληροφορία διαμοιράζεται σε όλη την κοινότητα χρηστών του CrowdSec, προστατεύοντας προληπτικά όλους τους άλλους από αυτήν την απειλή, ακόμα και αν δεν έχουν δεχθεί επίθεση από τη συγκεκριμένη IP.

## Η αρχιτεκτονική της λύσης

Όπως φαίνεται και στο διάγραμμα του οδηγού, η αρχιτεκτονική μας χρησιμοποιεί Docker για να απομονώσει τις υπηρεσίες:

![Εγκατάσταση και ασφάλεια του Nextcloud server με το CrowdSec](/post_images/nexcrowdsect/629481df6c5a0b48aaac1ba6_codimd.s3.shivering-isles.png "Εγκατάσταση και ασφάλεια του Nextcloud server με το CrowdSec"){:width="320px"}

- **User:** Ο χρήστης συνδέεται στον server. Η κίνησή του φτάνει πρώτα στον reverse proxy.
- **Openresty + Bouncer:** Είναι ο reverse proxy (βασισμένος σε Nginx) που δέχεται τα αιτήματα. Ενσωματώνει τον "bouncer" του CrowdSec. Είναι υπεύθυνος για την προώθηση της κίνησης στο Nextcloud και για το μπλοκάρισμα των κακόβουλων IPs.
- **Nextcloud & Database:** Ο πυρήνας της εφαρμογής και η βάση δεδομένων του.
- **Certbot:** Ένα container υπεύθυνο για την έκδοση και ανανέωση των δωρεάν πιστοποιητικών SSL/TLS από το Let's Encrypt.
- **CrowdSec:** Είναι ο "εγκέφαλος" του συστήματος. Δεν δέχεται απευθείας κίνηση. Διαβάζει συνεχώς τα logs που παράγονται από το Openresty και το Nextcloud, τα αναλύει, και όταν ανιχνεύσει επίθεση, ενημερώνει τον bouncer (μέσω του LAPI) να μπλοκάρει την IP του εισβολέα.

## Αναλυτικά Βήματα Εγκατάστασης

### 1. Προαπαιτούμενα

- Ένας server με **Debian 11 (ή νεότερο)**.
- Ένα όνομα τομέα (domain name) που να δείχνει ήδη στην IP του server σας.
- API keys για το reCAPTCHA V2 από τη Google (προαιρετικό, αλλά συνιστάται).

### 2. Δημιουργία χρήστη και Ασφάλεια SSH

#### Δημιουργία χρήστη με δικαιώματα sudo:

{% highlight ruby %}

# Δημιουργία του χρήστη (π.χ. bob) και ορισμός κωδικού

adduser bob --disabled-password
passwd bob

# Εγκατάσταση του sudo

apt-get -y install sudo

# Άνοιγμα αρχείου sudoers

nano /etc/sudoers
{% endhighlight %}

Και προσθήκη της γραμμής:

{% highlight ruby %}
bob ALL=(ALL:ALL) ALL
{% endhighlight %}

#### Αύξηση ασφάλειας του SSH:

{% highlight ruby %}

# Άνοιγμα αρχείου ρυθμίσεων SSH

sudo nano /etc/ssh/sshd_config
{% endhighlight %}

Τροποποιήστε ή προσθέστε τις παρακάτω γραμμές:

{% highlight ruby %}
Protocol 2
PermitRootLogin no
PermitEmptyPasswords no
MaxAuthTries 5
MaxSessions 2
{% endhighlight %}

Επανεκκίνηση υπηρεσίας SSH:

{% highlight ruby %}
sudo systemctl restart sshd.service
{% endhighlight %}

### 3. Ενημέρωση συστήματος & Firewall

{% highlight ruby %}

# Ενημέρωση πακέτων

sudo apt-get update
sudo apt-get -y dist-upgrade

# Εγκατάσταση firewall (ufw)

sudo apt-get install -y iptables iptables-persistent ufw
{% endhighlight %}

{% highlight ruby %}

# Άνοιγμα των απαραίτητων θυρών

sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https

# Ενεργοποίηση του firewall

sudo ufw enable
{% endhighlight %}

### 4. Εγκατάσταση Docker και Docker Compose

{% highlight ruby %}

# Εγκατάσταση απαραίτητων πακέτων

sudo apt-get install -y ca-certificates curl gnupg lsb-release

# Προσθήκη του GPG κλειδιού του Docker

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Προσθήκη του αποθετηρίου του Docker

echo \
 "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
 $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Εγκατάσταση Docker Engine

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Εγκατάσταση του Docker Compose

mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
{% endhighlight %}

### 5. Εγκατάσταση και Εκκίνηση των Nextcloud & CrowdSec

{% highlight ruby %}

# Εγκατάσταση git

sudo apt install -y git

# Κάνουμε clone το αποθετήριο

git clone https://github.com/Sykursen/crowdsec-nextcloud.git
{% endhighlight %}

Μεταβείτε στον φάκελο `crowdsec-nextcloud`. Επεξεργαστείτε τα αρχεία ρυθμίσεων αντικαθιστώντας το `example.org` με το domain σας και ορίζοντας ισχυρούς κωδικούς στο αρχείο `.env`.

{% highlight ruby %}
sudo docker compose up -d
{% endhighlight %}

### 6. Προσθήκη του Bouncer στο CrowdSec

{% highlight ruby %}

# Δημιουργία ενός νέου bouncer key

sudo docker exec nextcloud-crowdsec-1 cscli bouncers add openresty-nextcloud
{% endhighlight %}

Αντιγράψτε το API key που θα εμφανιστεί. Τώρα, επεξεργαστείτε το αρχείο ρυθμίσεων του bouncer και προσθέστε το κλειδί.

`/crowdsec/crowdsec-openresty-bouncer.conf.yaml`

{% highlight ruby %}
API*KEY: <Εδώ*το*κλειδί*που_αντιγράψατε>

# Επανεκκίνηση του bouncer για να εφαρμοστούν οι αλλαγές

sudo docker compose restart openresty
{% endhighlight %}

### 7. Ενεργοποίηση HTTPS με Let's Encrypt

{% highlight ruby %}

# Αντικαταστήστε το example.com με το δικό σας domain

sudo docker compose run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.com
{% endhighlight %}

Μόλις εκδοθεί το πιστοποιητικό, επεξεργαστείτε το αρχείο ρυθμίσεων του Openresty (`.conf`) και προσθέστε το μπλοκ server για την θύρα 443, όπως φαίνεται στον οδηγό. Θυμηθείτε να βάλετε τη σωστή διαδρομή για τα αρχεία του πιστοποιητικού σας.
{% highlight ruby %}

# ...

ssl_certificate /etc/nginx/ssl/live/example.com/fullchain.pem;
ssl_certificate_key /etc/nginx/ssl/live/example.com/privkey.pem;

# ...

{% endhighlight %}

Επανεκκινήστε για τελευταία φορά:
{% highlight ruby %}
sudo docker compose restart openresty
{% endhighlight %}

## Συμπέρασμα: Τι πετύχαμε;

Με την παραπάνω εγκατάσταση, δεν έχουμε απλώς έναν λειτουργικό Nextcloud server. Έχουμε δημιουργήσει μια **σκληροπυρηνική, αυτο-αμυνόμενη υποδομή** που προστατεύεται ενεργά. Το CrowdSec λειτουργεί ως άγρυπνος φρουρός, μπλοκάροντας αυτόματα προσπάθειες εισβολής (brute-force, scans κ.λπ.) πριν καν φτάσουν στην εφαρμογή σας.

Το πιο σημαντικό είναι ότι ο server σας δεν είναι πλέον μόνος του. Συμμετέχει σε ένα παγκόσμιο δίκτυο αμυνόμενων μηχανών. Κάθε φορά που ένας άλλος server, οπουδήποτε στον κόσμο, μπλοκάρει μια κακόβουλη IP, η δική σας εγκατάσταση μαθαίνει γι' αυτήν και την μπλοκάρει προληπτικά. Έτσι, ο Nextcloud σας είναι απείρως πιο ανθεκτικός σε απειλές.

Μην ξεχνάτε ότι τα πιστοποιητικά Let's Encrypt λήγουν κάθε 3 μήνες. Μπορείτε να τα ανανεώσετε με την εντολή:
{% highlight ruby %}
sudo docker compose run --rm certbot renew
{% endhighlight %}

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2025/08/nextcloud-server-crowdsec.html](https://eiosifidis.blogspot.com/2025/08/nextcloud-server-crowdsec.html){:target="\_blank"}
