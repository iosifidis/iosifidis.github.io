---
layout: post
title: "Εγκατάσταση Nextcloud AIO σε Linux με Docker"
date: 2025-02-08 12:00:00
description: Απλή και γρήγορη εγκατάσταση του Nextcloud All-in-One σε Linux με τη χρήση Docker. Mια βελτιωμένη εμπειρία χρήσης και ασφάλεια για το server σας.
tags:
- docker
- docker storage directory
categories:
- Greek
- DOCKER
twitter_text: 'Εγκατάσταση Nextcloud AIO σε Linux με Docker'
---

![Nextcloud AIO σε Linux με Docker](/post_images/nextcloud/aio/Nextcloud-AIO.webp "Nextcloud AIO σε Linux με Docker"){:width="320px"}

Αν είστε χρήστης Linux και θέλετε να εγκαταστήσετε το Nextcloud All-in-One (AIO), τότε αυτός ο οδηγός είναι για εσάς! Θα σας καθοδηγήσουμε βήμα προς βήμα στη διαδικασία εγκατάστασης του AIO χρησιμοποιώντας Docker, ώστε να έχετε μια πλήρως λειτουργική εγκατάσταση του Nextcloud με όλα τα απαραίτητα χαρακτηριστικά.  
  
## Τι είναι το Nextcloud All-in-One;

Το Nextcloud All-in-One (AIO) είναι μια ολοκληρωμένη λύση για την εύκολη εγκατάσταση και διαχείριση του Nextcloud. Πρόκειται για ένα Docker-based project που επιτρέπει την εγκατάσταση ενός κεντρικού container, το οποίο αναλαμβάνει να διαχειριστεί όλα τα υπόλοιπα containers που απαιτούνται για μια πλήρη εγκατάσταση του Nextcloud. Αυτό σημαίνει ότι δεν χρειάζεται να ρυθμίσετε μεμονωμένα τις εξαρτήσεις του Nextcloud, καθώς το AIO φροντίζει να τα διαχειρίζεται αυτόματα.  
  
Το AIO προσφέρει μια απλοποιημένη εμπειρία διαχείρισης, με εύκολες ενημερώσεις, ενσωματωμένα εργαλεία ασφαλείας και βελτιστοποιημένη απόδοση.  
  
## Από τι αποτελείται το Nextcloud AIO;

Το Nextcloud AIO περιλαμβάνει τα εξής modules και εργαλεία:

*   Nextcloud (η βασική πλατφόρμα cloud αποθήκευσης και συνεργασίας)   
*   Nextcloud Office (εργαλείο επεξεργασίας εγγράφων μέσα στο Nextcloud)   
*   Υψηλών επιδόσεων backend για Nextcloud Files (βελτιωμένη απόδοση για αρχεία)   
*   Υψηλών επιδόσεων backend για Nextcloud Talk (βελτιωμένη εμπειρία τηλεδιάσκεψης)   
*   Λύση δημιουργίας αντιγράφων ασφαλείας ([BorgBackup](https://github.com/borgbackup/borg#what-is-borgbackup?rel=iosifidis.gr)) (για προστασία δεδομένων)   
*   Imaginary (εργαλείο δημιουργίας προεπισκοπήσεων για αρχεία HEIC, TIFF και WebP)   
*   ClamAV (αντιϊκό backend για προστασία αρχείων)   
*   Fulltext search (μηχανή πλήρους αναζήτησης εγγράφων)   

Ένας από τους λόγους που το AIO ξεχωρίζει είναι η βελτιωμένη απόδοση, ιδιαίτερα σε επαγγελματικούς servers γραφείου.  
  
## Οδηγός εγκατάστασης Nextcloud AIO σε Linux

Προαπαιτούμενα Για να προχωρήσετε στην εγκατάσταση, θα χρειαστείτε:

*   Έναν υπολογιστή με Linux (π.χ. Ubuntu 22.04 LTS)   
*   Τουλάχιστον 4GB RAM και 2 πυρήνες CPU   
*   Ένα δημόσιο domain   
*   Δυνατότητα ανοίγματος ports (χωρίς CGNAT, καθώς αυτό θα δημιουργήσει προβλήματα στη λειτουργία)   

  
**Σημείωση:** Οι παρακάτω οδηγίες ισχύουν για εγκατάσταση χωρίς προϋπάρχον web server ή reverse proxy (Apache, Nginx, κ.λπ.). Αν επιθυμείτε εγκατάσταση πίσω από web server, ακολουθήστε τις σχετικές οδηγίες [εδώ](https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md?rel=iosifidis.gr).  
  
### 1\. Εγκατάσταση Docker

Το Nextcloud AIO λειτουργεί μέσω Docker, οπότε [πρέπει πρώτα να το εγκαταστήσετε](https://docs.docker.com/engine/install/#server.?rel=iosifidis.gr). Ο ευκολότερος τρόπος είναι μέσω του επίσημου script εγκατάστασης:

{% highlight ruby %}
curl -fsSL https://get.docker.com | sudo sh
{% endhighlight %}


Εάν χρειάζεστε υποστήριξη IPv6, ακολουθήστε τον οδηγό [εδώ](https://github.com/nextcloud/all-in-one/blob/main/docker-ipv6-support.mdhttps://docs.docker.com/engine/install/#server.?rel=iosifidis.gr).  
  

### 2\. Εγκατάσταση του Nextcloud AIO

Ανοίξτε το Terminal και εκτελέστε την ακόλουθη εντολή για να ξεκινήσετε το AIO container:

{% highlight ruby %}
sudo docker run \\
--sig-proxy=false \\
--name nextcloud-aio-mastercontainer \\
--restart always \\
--publish 80:80 \\
--publish 8080:8080 \\
--publish 8443:8443 \\
--volume nextcloud\_aio\_mastercontainer:/mnt/docker-aio-config \\
--volume /var/run/docker.sock:/var/run/docker.sock:ro \\
nextcloud/all-in-one:latest
{% endhighlight %}

**Σημαντικό:** Εάν θέλετε να αποθηκεύσετε τα αρχεία του Nextcloud σε διαφορετική τοποθεσία από το προεπιλεγμένο volume, δείτε τη σχετική τεκμηρίωση.  
  
### 3\. Πρόσβαση στο AIO interface

Αφού εκτελέσετε το container, ανοίξτε το AIO interface στη διεύθυνση:

[https://localhost:8080](https://localhost:8080) ή  
  
https://\[SERVER\_IP\]:8080

⚠️ Προσοχή: Χρησιμοποιήστε IP αντί για domain, καθώς το HSTS μπορεί να μπλοκάρει την πρόσβαση αργότερα. Πρέπει να αποδεχτείτε το πιστοποιητικό, στη συνέχεια θα πρέπει να δείτε αυτό:

![Nextcloud All-in-One](/post_images/nextcloud/aio/AIO-Linux.webp)

Εναλλακτικά, εάν η πόρτα 80 και 8443 πρέπει να είναι ανοικτές στο τείχος προστασίας/δρομολογητή σας και ένας τομέας έχει ρυθμιστεί ώστε να δείχνει στον διακομιστή σας, μπορείτε να φτάσετε στη διεπαφή AIO με έγκυρο πιστοποιητικό χρησιμοποιώντας **https://your-domain.com:8443**.  
  
Πατήστε "**Open Nextcloud AIO login**" και εισαγάγετε το συνθηματικό.

![Nextcloud All-in-One](/post_images/nextcloud/aio/AIO-Linux_2.webp)

Στη συνέχεια, θα πρέπει να δείτε τη διεπαφή AIO:

![Διεπαφή Nextcloud All-in-One](/post_images/nextcloud/aio/AIO-Linux_3.webp)

Στη συνέχεια, πληκτρολογήστε τον δημόσιο τομέα που έχετε πριν κάνετε αυτόν τον οδηγό. Η διεπαφή θα πρέπει να σας βοηθήσει να υπολογίσετε ποια είναι τα ακριβή βήματα. (Ρυθμίστε τα DDNs για τον τομέα σας για να δείξετε την δημόσια IP σας, το port-forward τουλάχιστον θύρες 443/TCP, 3478/UDP και 3478/TCP στη μηχανή Linux.  
  
Αφού το ρυθμίσετε σωστά, θα σας επιτρέψει να περάσετε στο επόμενο βήμα όπου μπορείτε να διαμορφώσετε τα επιθυμητά προαιρετικά πρόσθετα και τη ζώνη ώρας και κάντε κλικ στο **Start containers** για να τα κατεβάσετε και να ξεκινήσετε.

![Nextcloud All-in-One Start containers](/post_images/nextcloud/aio/AIO-Linux_4.webp)

Σε αυτό το σημείο, θα πρέπει να δείτε έναν spinner που θα διαρκέσει λίγο ανάλογα με την ταχύτητα του Διαδικτύου σας ~ περίπου 10 λεπτά ή περισσότερο.

![Nextcloud All-in-One spinner](/post_images/nextcloud/aio/AIO-Linux_5.webp)

Όταν όλα τα containers μεταφορτωθούν και ξεκινήσουν, θα δείτε αυτήν την οθόνη που εμφανίζει τα containers που εξακολουθούν να ξεκινούν τα οποία θα κάνουν την πρώτη εγκατάσταση για εσάς:

![Nextcloud All-in-One status](/post_images/nextcloud/aio/AIO-Linux_6.webp)

Όταν ολοκληρωθεί η εγκατάσταση, θα εμφανιστεί η αντίστοιχη οθόνη επιβεβαίωσης.

![Nextcloud All-in-One](/post_images/nextcloud/aio/AIO-Linux_7.webp)

Τέλος, μπορείτε τώρα να ανοίξετε τη νέα σας εμφάνιση NextCloud και να συνδεθείτε με τα συγκεκριμένα διαπιστευτήρια διαχειριστή.

### Συμπέρασμα

Με το Nextcloud All-in-One, η εγκατάσταση και διαχείριση του Nextcloud σε Linux γίνεται εξαιρετικά εύκολη. Αυτή η λύση προσφέρει μια βελτιωμένη εμπειρία χρήσης, απλοποιημένη διαχείριση και προσαρμόσιμες επιλογές ανάλογα με τις ανάγκες σας.  
  
Είστε έτοιμοι να αξιοποιήσετε πλήρως το Nextcloud στον server σας! 🚀 

Πηγή φωτογραφιών:    
[https://nextcloud.com/blog/how-to-install-the-nextcloud-all-in-one-on-linux/](https://nextcloud.com/blog/how-to-install-the-nextcloud-all-in-one-on-linux/?rel=iosifidis.gr)


Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2025/02/egatastasi-nextcloud-all-in-one.html](https://eiosifidis.blogspot.com/2025/02/egatastasi-nextcloud-all-in-one.html){:target="_blank"}
