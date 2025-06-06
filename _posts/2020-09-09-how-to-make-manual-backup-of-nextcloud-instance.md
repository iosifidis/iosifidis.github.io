---
layout: post
title: "Πως γίνεται χειροκίνητα ένα αντίγραφο ασφαλείας του Nextcloud Hub"
date: 2020-09-09 12:30:00
description: Το να έχεις στημένο το Nextcloud Hub δεν εξασφαλίζει τα δεδομένα σου. Αποθήκευσέ τα σε ένα μέρος ακόμα για σιγουριά.
tags:
- nextcloud
- nextcloud hub
- backup
- αντίγραφο ασφαλείας
- cloud
categories:
- NEXTCLOUD
- Greek
twitter_text: 'Πως να κάνετε χειροκίνητα ένα αντίγραφο ασφαλείας του #Nextcloud'
---

![Nextcloud Logo 3D](/post_images/nextcloud/Nextcloud_3D.jpg "Awesome Nextcloud")

# Για ποιο λόγο να κάνουμε αντίγραφο ασφαλείας;

Η ασφάλεια των δεδομένων, είναι το σημαντικότερο ζητούμενο την εποχή που ζούμε. Η έννοια της ασφάλειας αποτελείται τόσο από την πρόσβαση στα δεδομένα από τρίτους, όσο και από κάποιου είδους "ατύχημα" που θα οδηγήσει στην απώλεια σημαντικών δεδομένων. Όσον αφορά την πρόσβαση στα δεδομένα, το [Nextcloud Hub](https://eiosifidis.blogspot.com/search/label/nextcloud) εγγυάται ότι εάν ακολουθήσετε τις οδηγίες που δίνουν στα διάφορα εγχειρίδια στην ιστοσελίδα τους, δεν πρόκειται να δείτε δεδομένα σας σκόρπια στο διαδίκτυο. Όσον αφορά όμως την απώλεια σημαντικών δεδομένων, το Nextcloud εργάζεται ώστε να φτιάξει ένα εργαλείο όπου θα μπορείτε να φτιάχνετε ένα αντίγραφο ασφαλείας σε ένα άλλο χώρο. Μέχρι τότε πρέπει να γίνει χειροκίνητα από εσάς.

Μην ξεχνάτε τον κανόνα 3-2-1. Αυτό σημαίνει διατηρείστε τουλάχιστον τρία (3) αντίγραφα των δεδομένων σας και αποθηκεύστε δύο (2) αντίγραφα ασφαλείας σε διαφορετικά μέσα αποθήκευσης, με ένα (1) από αυτά να βρίσκονται εκτός του χώρου του σπιτιού σας.<br />

Επίσης μην ξεχνάτε την έννοια του [Schrödinger’s](https://el.wikipedia.org/wiki/%CE%88%CF%81%CE%B2%CE%B9%CE%BD_%CE%A3%CF%81%CE%AD%CE%BD%CF%84%CE%B9%CE%BD%CE%B3%CE%BA%CE%B5%CF%81) Backup. Αυτό σημαίνει ότι όταν κάνετε ένα αντίγραφο ασφαλείας, δοκιμάστε το εάν δουλεύει όντως. Διότι υπάρχει περίπτωση να κρατάτε τζάμπα τα αντίγραφα ασφαλείας. Στην περίπτωση αυτή θα πρέπει να υπάρχει κάπου εύκαιρος ένας δεύτερος διακομιστής όπου θα το δοκιμάζετε. Όλα αυτά μπορούν να γίνουν εύκολα με τεχνολογίες εικομικοποίησης (docker-podman κλπ).

Αρκετός πρόλογος. Ας δούμε την δράση.

# Τι θα χρειαστούμε

* Μια εγκατάσταση του Nextcloud Hub
* Τον χρήστη με δικαιώματα διαχειριστή (sudo)
* Χώρο που θα αποθηκεύσουμε το αντίγραφο ασφαλείας

Υποθέτουμε ότι η εγκατάσταση είναι στον φάκελο **/var/www/html/nextcloud**. Εάν εσείς το έχετε εγκατεστημένο σε άλλο φάκελο, διορθώστε τις παρακάτω εντολές.

# Αλλαγή σε λειτουργία συντήρησης

Πρώτη κίνηση είναι να αλλάξουμε την λειτουργία του Nextcloud σε λειτουργία συντήρησης. Για να το κάνετε αυτό, μετά την είσοδο στον διακομιστή, εκτελέστε τις παρακάτω εντολές:

{% highlight ruby %}
  cd /var/www/html/nextcloud
  sudo -u www-data php occ maintenance:mode --on
{% endhighlight %}

# Δημιουργία αντιγράφου ασφαλείας των φακέλων

Στη συνέχεια, υπάρχουν αρκετοί φάκελοι και αρχεία προς δημιουργία αντιγράφων ασφαλείας. Ωστόσο, αντί να δημιουργούμε αντίγραφα ασφαλείας ξεχωριστά, θα δημιουργήσουμε αντίγραφα ασφαλείας ολόκληρου του φακέλου Nextcloud χρησιμοποιώντας το rsync. Εδώ θα χρειαστείτε μια δευτερεύουσα τοποθεσία για τα δεδομένα. Αυτό θα γίνει βήμα-βήμα:

Δημιουργήστε το αντίγραφο ασφαλείας με τις παρακάτω εντολές:

{% highlight ruby %}
cd /var/www/html/
sudo rsync -Aavx nextcloud/ /LOCATION/nextcloud-backup_`date +"%Y%m%d"`/
{% endhighlight %}

Όπου LOCATION είναι ο κατάλογος για την αποθήκευση της εγκατάστασης Nextcloud. Ανάλογα με το πόσα δεδομένα έχετε σε αυτόν τον κατάλογο, αυτό μπορεί να διαρκέσει αρκετά.

Συμπιέστε το αντίγραφο ασφαλείας με την παρακάτω εντολή:

{% highlight ruby %}
tar cfz /LOCATION/nextcloud-backup_DATE.tgz /LOCATION/nextcloud-backup_DATE/
{% endhighlight %}

Όπου LOCATION είναι η τοποθεσία που αποθήκευσης του αντίγραφου ασφαλείας και DATE είναι η ημερομηνία που επισυνάπτεται στο όνομα αρχείου.

# Δημιουργία αντιγράφου ασφαλείας της βάσης δεδομένων

Οι κατάλογοι δεν είναι το μόνο πράγμα που πρέπει να αποθηκευτεί με το αντιγράφο ασφαλείας. Πρέπει επίσης να δημιουργήσουμε αντίγραφα ασφαλείας της βάσης δεδομένων μας. Τα παρακάτω ισχύουν με την MySQL ή με MariaDB. Για να δημιουργήσετε αντίγραφα ασφαλείας της βάσης δεδομένων, εκτελέστε την εντολή:

{% highlight ruby %}
sudo mysqldump --single-transaction -h SERVER -u USER -p nextcloud > nextclouddb-backup_`date +"%Y%m%d"`.bak
{% endhighlight %}

Όπου SERVER είναι η τοποθεσία της βάσης δεδομένων - εάν φιλοξενείται στον ίδιο υπολογιστή με το Nextcloud Hub, θα είναι localhost - και ο USER είναι χρήστης με δικαιώματα διαχειριστή MySQL.

Μόλις έχετε τόσο τη βάση δεδομένων σας όσο και τα αντίγραφα ασφαλείας του καταλόγου σας, τοποθετήστε τα σε μια ασφαλή τοποθεσία.

Πιθανότατα θα πρέπει να λαμβάνετε τακτικά αντίγραφα ασφαλείας τόσο του καταλόγου Nextcloud όσο και της βάσης δεδομένων σας. Επομένως, σκεφτείτε να δημιουργήσετε μια εργασία cron για αυτήν την εργασία (βάζοντας και τις δύο εντολές σε ένα σενάριο).

# Έξοδος από την λειτουργία συντήρησης

Τώρα που αποθηκεύσατε τα αντίγραφα ασφαλείας, επαναφέρετε τον διακομιστή Nextcloud Hub σε κανονική λειτουργία με τις εντολές:

{% highlight ruby %}
cd /var/www/html/nextcloud
sudo -u www-data php occ maintenance:mode --off
{% endhighlight %}

Συγχαρητήρια, δημιουργήθηκε αντίγραφο ασφαλείας του διακομιστή Nextcloud. Τα δεδομένα σας, τα έχετε αποθηκευμένα σε διαφορετικό δίσκο.

Επόμενο βήμα είναι η δοκιμή για επαναφορά του αντιγράφου ασφαλείας, για να δείτε ότι μεταφέρθηκε σωστά.
