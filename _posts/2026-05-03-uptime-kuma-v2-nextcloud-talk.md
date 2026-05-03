---
layout: post
title: "Ενσωμάτωση Uptime Kuma v2 με Nextcloud Talk: Λαμβάνετε ειδοποιήσεις monitoring απευθείας στο chat σας"
date: 2026-05-03 12:00:00
description: Οδηγός ρύθμισης Uptime Kuma v2 με Nextcloud Talk. Λαμβάνετε αυτόματες ειδοποιήσεις monitoring στο chat σας μέσω Talk bot σε Nextcloud AIO.
tags:
  - nextcloud
  - monitoring
  - docker
  - uptime kuma
categories:
  - Greek
  - NEXTCLOUD
  - UPTIME KUMA
twitter_text: "Ενσωμάτωση Uptime Kuma v2 με Nextcloud Talk: Λαμβάνετε ειδοποιήσεις monitoring απευθείας στο chat σας"
---

![Ενσωμάτωση Uptime Kuma v2 με Nextcloud Talk](/post_images/nextcloud/uptime-nextcloud.webp){:width="320px"}

Αν διαχειρίζεστε υπηρεσίες στον δικό σας server — είτε πρόκειται για Nextcloud, [Audiobookshelf](https://github.com/advplyr/audiobookshelf), Plex, Proxmox ή οτιδήποτε άλλο — γνωρίζετε πόσο σημαντικό είναι να μαθαίνετε _αμέσως_ όταν κάτι πάει στραβά. Η αναμονή να ανακαλύψετε τυχαία ότι μια υπηρεσία «έπεσε» δεν είναι διαχείριση — είναι τύχη.

Σε αυτό το άρθρο θα δούμε πώς να συνδέσουμε το **[Uptime Kuma v2](https://uptime.kuma.pet/)** με το **[Nextcloud Talk](https://nextcloud.com/talk/)**, ώστε κάθε φορά που μια υπηρεσία αποτυγχάνει ή ανακάμπτει, να λαμβάνετε αυτόματη ειδοποίηση απευθείας σε μια συνομιλία του Talk. Όλα αυτά χωρίς εξωτερικές υπηρεσίες, χωρίς συνδρομές, 100% open source.

## 🐻 Τι είναι το Uptime Kuma;

Το **Uptime Kuma** είναι ένα ελεύθερο, ανοιχτού κώδικα εργαλείο παρακολούθησης υπηρεσιών (monitoring), εμπνευσμένο από υπηρεσίες όπως το [UptimeRobot](https://uptimerobot.com/). Διαθέτει φιλικό web interface μέσω του οποίου μπορείτε να παρακολουθείτε HTTP endpoints, TCP ports, DNS, υπηρεσίες Docker, ping και πολλά ακόμα. Το **Uptime Kuma v2** — που κυκλοφόρησε το 2025 — έφερε σημαντικές βελτιώσεις στη βάση δεδομένων, στα notifications και, μεταξύ άλλων, **ενσωματωμένη υποστήριξη για Nextcloud Talk** ως πάροχο ειδοποιήσεων. Τρέχει εύκολα μέσω Docker και αποτελεί την ιδανική επιλογή για αυτόνομες εγκαταστάσεις.

## Πώς λειτουργεί η ενσωμάτωση

Η επικοινωνία μεταξύ Uptime Kuma και Nextcloud Talk γίνεται μέσω ενός **Talk Bot**: το Uptime Kuma στέλνει HTTP αιτήματα υπογεγραμμένα με ένα shared secret απευθείας στο Talk API, και το bot δημοσιεύει το μήνυμα στην επιλεγμένη συνομιλία. Δεν απαιτείται κανένας ενδιάμεσος server ή webhook service.

{% highlight ruby %}
Uptime Kuma
    │
    │  HTTP αίτημα (υπογεγραμμένο με HMAC-SHA256)
      ▼
Nextcloud Talk Bot API
    │
      ▼
Talk conversation (π.χ. infra-alerts)
{% endhighlight %}

Τα μηνύματα που λαμβάνετε έχουν αυτή τη μορφή:

`[Metabase] 🔴 Down — Request failed with status code 502`

`[Metabase] ✅ Up — 200 OK`

## Προαπαιτούμενα

Πριν ξεκινήσουμε, βεβαιωθείτε ότι έχετε:

- **Nextcloud 32.x** εγκατεστημένο μέσω **Nextcloud AIO** (All-in-One Docker)
- Την εφαρμογή **Nextcloud Talk** εγκατεστημένη και ενεργοποιημένη
- **Uptime Kuma v2** σε λειτουργία (π.χ. μέσω Docker)
- Διαχειριστική πρόσβαση στο Nextcloud (admin account)
- Πρόσβαση σε τερματικό SSH στον server σας

> **⚠️ Σημείωση για το Talk Bot API:** Το Nextcloud Talk Bot API απαιτεί ελάχιστο **Talk 17.1** (συμβατό με Nextcloud 27.1+). Στο Nextcloud 32 είναι πλήρως διαθέσιμο. Για λόγους ασφαλείας, τα bots μπορούν να εγκατασταθούν _μόνο μέσω γραμμής εντολών_ (occ commands) — δεν υπάρχει γραφική διεπαφή εγκατάστασης bot.

## 1 Δημιουργία συνομιλίας στο Nextcloud Talk

Το πρώτο βήμα είναι να δημιουργήσουμε μια αποκλειστική συνομιλία (conversation / room) στο Talk, μέσα στην οποία θα αποστέλλονται οι ειδοποιήσεις monitoring. Συνιστούμε να μην χρησιμοποιείτε υπάρχουσα ομαδική συνομιλία, για να κρατάτε τις ειδοποιήσεις οργανωμένες.

1. Συνδεθείτε στη διεπαφή web του Nextcloud.
2. Ανοίξτε την εφαρμογή **Talk**.
3. Κάντε κλικ στο **«Νέα συνομιλία»** (New conversation).
4. Δώστε ένα περιγραφικό όνομα, π.χ. `infra-alerts` ή `monitoring`.
5. Δημιουργήστε τη συνομιλία.

## 2 Εύρεση του Conversation Token

Κάθε συνομιλία στο Talk ταυτοποιείται με ένα μοναδικό **token** που εμφανίζεται στη διεύθυνση URL της συνομιλίας. Αυτό το token θα το χρειαστούμε αργότερα για να «πούμε» στο Uptime Kuma πού να στέλνει τις ειδοποιήσεις.

Αφού ανοίξετε τη νέα συνομιλία, κοιτάξτε τη διεύθυνση URL στον browser σας. Θα μοιάζει κάπως έτσι:

`https://cloud.example.com/call/abcd1234`

Το τμήμα μετά το `/call/` είναι το **Conversation Token** — στο παράδειγμά μας το `abcd1234`. Αντιγράψτε το και κρατήστε το.

## 3 Δημιουργία του Talk Bot

Τώρα πρέπει να δημιουργήσουμε το bot που θα στέλνει μηνύματα στη συνομιλία. Αυτό γίνεται αποκλειστικά μέσω **γραμμής εντολών**, μέσα στο Docker container του Nextcloud AIO.

Το Nextcloud Talk προσφέρει δύο εντολές για τη δημιουργία bot:

- `talk:bot:create` — δημιουργεί bot με αυτόματα παραγόμενο secret, με τη δυνατότητα `response` μόνο (το bot μπορεί να στέλνει μηνύματα στο chat).
- `talk:bot:install` — πιο προχωρημένη εντολή που απαιτεί και webhook URL, για bots που θέλουν επίσης να _λαμβάνουν_ μηνύματα.

Για το Uptime Kuma, αρκεί η `talk:bot:create`, γιατί το bot μόνο **αποστέλλει** ειδοποιήσεις — δεν χρειάζεται να λαμβάνει ή να απαντά σε μηνύματα. Εκτελέστε την παρακάτω εντολή:

{% highlight ruby %}
sudo docker exec -it nextcloud-aio-nextcloud \
  php occ talk:bot:create "Uptime Kuma Bot"
{% endhighlight %}

Μπορείτε επίσης να ορίσετε εσείς το secret (πρέπει να είναι μεταξύ 40 και 128 χαρακτήρων) χρησιμοποιώντας την παράμετρο `--secret`:

{% highlight ruby %}
sudo docker exec -it nextcloud-aio-nextcloud \
  php occ talk:bot:create "Uptime Kuma Bot" --secret YOUR_SECRET_HERE
{% endhighlight %}

Αν _δεν_ ορίσετε secret, το Nextcloud θα δημιουργήσει αυτόματα ένα τυχαίο string 64 χαρακτήρων. Το αποτέλεσμα θα μοιάζει κάπως έτσι:

{% highlight ruby %}
Bot created successfully
Bot ID: 16
Secret: 3cfa8b8c5e4e1f7b7d2c2a0c0a9c0e4f7c8a3b9e2d1c6f5a4b3c2d1e0f9a8b7
{% endhighlight %}

> **✅ Σημαντικό:** Αποθηκεύστε αμέσως το **Bot ID** και το **Bot Secret**. Το secret δεν εμφανίζεται ξανά από το σύστημα — αν το χάσετε, πρέπει να διαγράψετε και να ξαναδημιουργήσετε το bot.

## 4 Προσθήκη του Bot στη συνομιλία

Το bot δημιουργήθηκε στο σύστημα, αλλά δεν έχει ακόμα πρόσβαση σε καμία συνομιλία. Πρέπει να το «συνδέσουμε» με τη συνομιλία που δημιουργήσαμε. Αυτό γίνεται με την εντολή `talk:bot:setup`, η οποία δέχεται το **Bot ID** και το **Conversation Token**:

{% highlight ruby %}
sudo docker exec -it nextcloud-aio-nextcloud \
  php occ talk:bot:setup 16 abcd1234
{% endhighlight %}

Αντικαταστήστε `16` με το πραγματικό Bot ID σας και `abcd1234` με το Conversation Token της συνομιλίας σας. Αν η εντολή εκτελεστεί σωστά, θα δείτε:

`Successfully set up for conversation abcd1234`

Το bot είναι τώρα μέλος της συνομιλίας και έχει δικαίωμα αποστολής μηνυμάτων.

## 5 Επαλήθευση της ρύθμισης του Bot

Για να επιβεβαιώσουμε ότι το bot εγκαταστάθηκε σωστά, μπορούμε να εκτελέσουμε την εντολή `talk:bot:list` που εμφανίζει όλα τα bots του συστήματος:

{% highlight ruby %}
sudo docker exec -it nextcloud-aio-nextcloud \
  php occ talk:bot:list
{% endhighlight %}

Παράδειγμα εξόδου:

{% highlight ruby %}
ID  | Name            | features
16  | Uptime Kuma Bot | response
{% endhighlight %}

Η στήλη `features` δείχνει **response**, που σημαίνει ότι το bot μπορεί να δημοσιεύει μηνύματα στις συνομιλίες — ακριβώς αυτό που χρειαζόμαστε για το Uptime Kuma.

Μπορείτε επίσης να φιλτράρετε τη λίστα για μια συγκεκριμένη συνομιλία περνώντας το token ως παράμετρο: `php occ talk:bot:list abcd1234`. Αυτό είναι χρήσιμο αν έχετε πολλά bots και θέλετε να δείτε ποια είναι ενεργά σε συγκεκριμένη συνομιλία.

## 6 Ρύθμιση notification στο Uptime Kuma

Ανοίξτε τη web διεπαφή του Uptime Kuma και μεταβείτε στις ρυθμίσεις ειδοποιήσεων:

`⚙️ Settings → Notifications → Add New Notification`

Στη λίστα τύπων notification, επιλέξτε **«Nextcloud Talk»**. Θα εμφανιστεί φόρμα με τα εξής πεδία:

| Πεδίο | Τιμή | Σχόλιο |
|---|---|---|
| **Friendly Name** | `Nextcloud Talk Alerts` | Όνομα ειδοποίησης (εσωτερικό στο Uptime Kuma) |
| **Nextcloud Host** | `https://cloud.example.com` | Η διεύθυνση του Nextcloud σας (_χωρίς_ trailing slash) |
| **Conversation Token** | `abcd1234` | Το token από το Βήμα 2 |
| **Bot Secret** | `3cfa8b8c5e4e1f7b...` | Το secret από το Βήμα 3 |

Αποθηκεύστε τη ρύθμιση κάνοντας κλικ στο **«Save»**.

## 7 Δοκιμή της ειδοποίησης

Κάντε κλικ στο κουμπί **«Test»** δίπλα στη νέα ειδοποίηση. Αν όλα έχουν ρυθμιστεί σωστά, σε λίγα δευτερόλεπτα θα εμφανιστεί ένα δοκιμαστικό μήνυμα στη συνομιλία του Talk:

`My Nextcloud Talk Alert Testing`

Αν δεν εμφανιστεί μήνυμα, ελέγξτε:

- Ότι το Bot Secret που πληκτρολογήσατε είναι ακριβώς το ίδιο με αυτό που παράχθηκε (χωρίς κενά).
- Ότι το Conversation Token αντιστοιχεί στη σωστή συνομιλία.
- Ότι η διεύθυνση Nextcloud Host είναι προσβάσιμη από τον server του Uptime Kuma (DNS, firewall, κ.λπ.).
- Ότι εκτελέσατε το `talk:bot:setup` με το σωστό token.

## 8 Ανάθεση της ειδοποίησης σε monitors

Η ειδοποίηση δεν ενεργοποιείται αυτόματα για όλα τα monitors. Πρέπει να την αναθέσετε ξεχωριστά σε κάθε monitor που θέλετε να παρακολουθείτε:

1. Ανοίξτε ένα monitor στο Uptime Kuma.
2. Κάντε κλικ στο **«Edit»**.
3. Κυλήστε κάτω στην ενότητα **«Notifications»**.
4. Επιλέξτε **«Nextcloud Talk Alerts»** (ή το όνομα που δώσατε).
5. Αποθηκεύστε τις αλλαγές.

Από εδώ και πέρα, κάθε φορά που η κατάσταση αυτού του monitor αλλάζει (Down ↔ Up), το Uptime Kuma θα στέλνει αυτόματα ειδοποίηση στο Talk.

## 💡 Συμβουλές για καλύτερες ειδοποιήσεις

Η ποιότητα μιας ειδοποίησης εξαρτάται σε μεγάλο βαθμό από το όνομα που δίνετε στο monitor. Αντί για γενικά ονόματα όπως «Server 1» ή «HTTP Check», χρησιμοποιήστε περιγραφικά ονόματα που σας λένε αμέσως ποια υπηρεσία επηρεάζεται. Χρήση emoji βοηθά στην οπτική γρήγορη αναγνώριση:

`🖥 Proxmox` &nbsp; `🎬 Plex` &nbsp; `📚 Audiobookshelf` &nbsp; `💾 PBS Backup` &nbsp; `☁️ Nextcloud`

Με αυτά τα ονόματα, οι ειδοποιήσεις στο Talk θα εμφανίζονται έτσι:

`[🎬 Plex] 🔴 Down — Request failed with status code 502`

`[🎬 Plex] ✅ Up — 200 OK`

Είναι αμέσως κατανοητό ποια υπηρεσία έχει πρόβλημα, χωρίς να χρειαστεί να ανοίξετε το dashboard του Uptime Kuma.

## 🔧 Χρήσιμες εντολές διαχείρισης

Αν χρειαστεί να διαχειριστείτε τα bots σας στο μέλλον, οι παρακάτω εντολές θα σας φανούν χρήσιμες:

{% highlight ruby %}
# Λίστα όλων των bots
php occ talk:bot:list

# Λίστα bots σε συγκεκριμένη συνομιλία
php occ talk:bot:list abcd1234

# Αφαίρεση bot από συνομιλία (χωρίς απεγκατάσταση)
php occ talk:bot:remove 16 abcd1234

# Πλήρης απεγκατάσταση bot από το σύστημα
php occ talk:bot:uninstall 16
{% endhighlight %}

Θυμηθείτε να προσθέτετε πάντα το prefix `sudo docker exec -it nextcloud-aio-nextcloud` πριν από κάθε `php occ ...` εντολή όταν χρησιμοποιείτε Nextcloud AIO.

## Συμπέρασμα

Η ενσωμάτωση Uptime Kuma v2 με Nextcloud Talk είναι μια 100% open source, self-hosted λύση που σας επιτρέπει να λαμβάνετε ειδοποιήσεις monitoring απευθείας στο chat σας — χωρίς εξωτερικές υπηρεσίες, χωρίς κόστος.

Με λίγες εντολές OCC και μια σύντομη ρύθμιση στο Uptime Kuma, έχετε ένα πλήρες σύστημα ειδοποιήσεων που σας ενημερώνει αμέσως για κάθε αλλαγή κατάστασης στις υπηρεσίες σας.


**Αρχική δημοσίευση**: 

[https://eiosifidis.blogspot.com/2026/05/uptime-kuma-v2-nextcloud-talk.html](https://eiosifidis.blogspot.com/2026/05/uptime-kuma-v2-nextcloud-talk.html){:target="\_blank"}
