---
layout: post
title: "Πώς να ρυθμίσετε το DuckDNS στο Raspberry Pi για απομακρυσμένη πρόσβαση"
date: 2026-01-18 12:00:00
description: Μάθετε πώς να ρυθμίσετε το DuckDNS στο Raspberry Pi. Δωρεάν Dynamic DNS για απομακρυσμένη πρόσβαση. Βήμα προς βήμα εγκατάσταση με cron.
tags:
  - duckdns
  - raspberry pi
  - ddns
  - cron
  - devops
categories:
  - Greek
  - DUCKDNS
  - RASPBERRY PI
  - DEVOPS
twitter_text: "Πώς να ρυθμίσετε το DuckDNS στο Raspberry Pi για απομακρυσμένη πρόσβαση"
---

![DuckDNS στο Raspberry Pi](/post_images/devops/duckdns.webp "DuckDNS στο Raspberry Pi"){:width="500px"}

Το DuckDNS είναι μια δωρεάν υπηρεσία Dynamic DNS που σας επιτρέπει να έχετε σταθερή πρόσβαση στο [Raspberry Pi](https://www.raspberrypi.com/) σας, ακόμα και αν η δημόσια IP διεύθυνσή σας αλλάζει. Σε αυτόν τον οδηγό, θα δείτε βήμα προς βήμα πώς να ρυθμίσετε το DuckDNS στο Raspberry Pi σας.

## 1. Δημιουργία Λογαριασμού και Domain στο DuckDNS

Το πρώτο βήμα είναι να δημιουργήσετε έναν δωρεάν λογαριασμό στο DuckDNS και να καταχωρήσετε το domain σας.

Επισκεφθείτε την ιστοσελίδα του **DuckDNS** ([duckdns.org](https://duckdns.org)) και συνδεθείτε χρησιμοποιώντας έναν από τους υποστηριζόμενους τρόπους όπως Google, Twitter, GitHub ή Reddit. Αφού συνδεθείτε, δημιουργήστε ένα νέο subdomain, για παράδειγμα `τοονομαμου.duckdns.org`. Αυτό το subdomain θα χρησιμοποιείται για την απομακρυσμένη πρόσβαση στο Raspberry Pi σας.

**Σημαντικό:** Σημειώστε το token (κλειδί API) που θα βρείτε στον λογαριασμό σας, καθώς θα το χρειαστείτε για το script ενημέρωσης.

## 2. Προετοιμασία του Raspberry Pi

Συνδεθείτε στο Raspberry Pi σας μέσω SSH. Πρώτα, βεβαιωθείτε ότι το σύστημά σας είναι πλήρως ενημερωμένο:

{% highlight ruby %}
sudo apt update
sudo apt upgrade -y
{% endhighlight %}

Εγκαταστήστε το `curl` εάν δεν είναι ήδη εγκατεστημένο, καθώς θα χρησιμοποιηθεί για την αποστολή των αιτημάτων ενημέρωσης στο DuckDNS:

{% highlight ruby %}
sudo apt install curl -y
{% endhighlight %}

Δημιουργήστε έναν κατάλογο για τα αρχεία του DuckDNS script:

{% highlight ruby %}
sudo mkdir /opt/duckdns/
{% endhighlight %}

Και έναν κατάλογο για τα αρχεία καταγραφής (logs):

{% highlight ruby %}
sudo mkdir /var/log/duckdns/
{% endhighlight %}

## 3. Δημιουργία του Script Ενημέρωσης

Τώρα θα δημιουργήσετε το script που θα ενημερώνει αυτόματα την IP διεύθυνσή σας στο DuckDNS.

Δημιουργήστε ένα αρχείο script με το όνομα `duck.sh`:

{% highlight ruby %}
sudo nano /opt/duckdns/duck.sh
{% endhighlight %}

Προσθέστε τις ακόλουθες γραμμές στο αρχείο, αντικαθιστώντας `<YOUR_DOMAIN>` με το subdomain σας (π.χ., `τοονομαμου`) και `<YOUR_TOKEN>` με το token που λάβατε από το DuckDNS:

{% highlight ruby %}
echo url="https://www.duckdns.org/update?domains=<YOUR_DOMAIN>&token=<YOUR_TOKEN>&ip=" | curl -k -o /var/log/duckdns/duck.log -K -
{% endhighlight %}

Αποθηκεύστε το αρχείο πατώντας `Ctrl+X`, μετά `Y` και `Enter`.

**Συμβουλή:** Μπορείτε να προσθέσετε πολλά domains στο ίδιο script διαχωρίζοντάς τα με κόμμα, π.χ.: `domains=domain1,domain2`

## Πολλαπλά Domains στο Ίδιο Script

Εάν διαχειρίζεστε περισσότερα από ένα domains στο DuckDNS, μπορείτε να τα ενημερώνετε όλα με ένα μόνο script.

Για παράδειγμα, αν έχετε δύο domains, `τοονομαμου.duckdns.org` και `τοδευτεροονομαμου.duckdns.org`, το script `duck.sh` θα γίνει ως εξής:

{% highlight ruby %}
echo url="https://www.duckdns.org/update?domains=<YOUR_DOMAIN1>,<YOUR_DOMAIN2>&token=<YOUR_TOKEN>&ip=" | curl -k -o /var/log/duckdns/duck.log -K -
{% endhighlight %}

Αντικαταστήστε:

*   `<YOUR_DOMAIN1>` με το πρώτο σας domain (π.χ., `τοονομαμου`)   
*   `<YOUR_DOMAIN2>` με το δεύτερο σας domain (π.χ., `τοδευτεροονομαμου`)   
*   `<YOUR_TOKEN>` με το token σας από το DuckDNS

Με αυτόν τον τρόπο, με μία μόνο κλήση του script, θα ενημερωθούν όλες οι καταχωρήσεις domain που έχετε ορίσει.

## 4. Ορισμός Δικαιωμάτων για το Script

Δώστε δικαιώματα εκτέλεσης στο script για λόγους ασφαλείας:

{% highlight ruby %}
sudo chmod 700 /opt/duckdns/duck.sh
{% endhighlight %}

Αυτό διασφαλίζει ότι μόνο ο root χρήστης μπορεί να διαβάσει, να γράψει και να εκτελέσει το script.

## 5. Αυτόματη Εκτέλεση του Script με το Cron

Για να ενημερώνεται αυτόματα η IP διεύθυνσή σας στο DuckDNS, θα προγραμματίσετε την εκτέλεση του script κάθε πέντε λεπτά χρησιμοποιώντας το **cron**.

Ανοίξτε το crontab:

{% highlight ruby %}
sudo crontab -e
{% endhighlight %}

Προσθέστε την ακόλουθη γραμμή στο τέλος του αρχείου, η οποία θα εκτελεί το script κάθε 5 λεπτά:

{% highlight ruby %}
*/5 * * * * /opt/duckdns/duck.sh >/dev/null 2>&1
{% endhighlight %}

Αποθηκεύστε το αρχείο πατώντας `Ctrl+X`, μετά `Y` και `Enter`.

**Τι σημαίνει η εντολή cron:**

*   `*/5` - Κάθε 5 λεπτά   
*   `* * * *` - Κάθε ώρα, μέρα, μήνα και ημέρα εβδομάδας   
*   `>/dev/null 2>&1` - Καταστολή εξόδου

## 6. Έλεγχος Λειτουργίας

Μπορείτε να δοκιμάσετε να εκτελέσετε το script χειροκίνητα για να επαληθεύσετε ότι λειτουργεί σωστά:

{% highlight ruby %}
sudo /opt/duckdns/duck.sh
{% endhighlight %}

Στη συνέχεια, ελέγξτε το αρχείο καταγραφής για να δείτε το αποτέλεσμα:

{% highlight ruby %}
cat /var/log/duckdns/duck.log
{% endhighlight %}

**Επιτυχής ενημέρωση:** Εάν δείτε την απάντηση `OK`, σημαίνει ότι η ενημέρωση ήταν επιτυχής και το DuckDNS λειτουργεί κανονικά.

**Αποτυχία ενημέρωσης:** Εάν δείτε `KO`, υπάρχει κάποιο σφάλμα στο script, στα δικαιώματα ή στο token που χρησιμοποιήσατε. Ελέγξτε ξανά τις παραμέτρους σας.

## 7. Ρύθμιση Port Forwarding στο Router (Προαιρετικό)

Για να έχετε πλήρη απομακρυσμένη πρόσβαση στο Raspberry Pi σας από το Internet, θα χρειαστεί να ρυθμίσετε το **port forwarding** (προώθηση θυρών) στον router σας.

Η διαδικασία αυτή διαφέρει ανάλογα με τον κατασκευαστή και το μοντέλο του router σας, οπότε συμβουλευτείτε το εγχειρίδιο χρήσης του. Γενικά, θα πρέπει να:

*   Συνδεθείτε στη διεπαφή διαχείρισης του router σας   
*   Βρείτε την ενότητα Port Forwarding ή Virtual Server   
*   Προωθήστε τις απαραίτητες θύρες (π.χ., 22 για SSH, 80 για HTTP, 443 για HTTPS) στην τοπική IP διεύθυνση του Raspberry Pi σας

**Σημαντική συμβουλή ασφαλείας:** Αλλάξτε την προεπιλεγμένη θύρα SSH (22) σε μια άλλη για να αυξήσετε την ασφάλεια του συστήματός σας.

## Συμπέρασμα

Με αυτόν τον οδηγό, μάθατε πώς να ρυθμίσετε το DuckDNS στο Raspberry Pi σας για να έχετε σταθερή απομακρυσμένη πρόσβαση, ακόμα και με δυναμική IP διεύθυνση. Το DuckDNS είναι μια αξιόπιστη και δωρεάν λύση που ενημερώνει αυτόματα το DNS κάθε 5 λεπτά μέσω του cron job που δημιουργήσατε.

Τώρα μπορείτε να έχετε πρόσβαση στο Raspberry Pi σας από οπουδήποτε στον κόσμο χρησιμοποιώντας το domain σας αντί για την IP διεύθυνση!

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2026/01/duckdns-raspberry-pi.html](https://eiosifidis.blogspot.com/2026/01/duckdns-raspberry-pi.html){:target="_blank"}
