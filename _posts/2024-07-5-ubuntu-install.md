---
layout: post
title: "Ubuntu 24.04: Οδηγός εγκατάστασης με τον βελτιωμένο εγκαταστάτη"
date: 2024-07-05 10:00:00
description: Ο νέος εγκαταστάτης του Ubuntu γραμμένος σε Flutter
tags:
  - Ubuntu
  - Flutter
  - "24.04"
  - Εγκατάσταση
categories:
  - Greek
  - UBUNTU
twitter_text: "Ubuntu 24.04: Οδηγός εγκατάστασης με τον βελτιωμένο εγκαταστάτη"
---

![Ubuntu Logo](/post_images/ubuntu/Ubuntu-Logo.png "Ubuntu Logo"){:width="320px"}

Το Ubuntu 24.04 LTS (Noble Numbat) κυκλοφόρησε τον Απρίλιο του 2024, φέρνοντας μαζί του μια σειρά από νέα χαρακτηριστικά και βελτιώσεις. Μία από τις πιο σημαντικές αλλαγές είναι ο ανανεωμένος εγκαταστάτης (είχε ξεκινήσει από την προηγούμενη έκδοση και είναι υλοποιημένος σε Flutter), ο οποίος έχει σχεδιαστεί για να κάνει την εγκατάσταση του λειτουργικού συστήματος πιο εύκολη και φιλική προς τον χρήστη από ποτέ.

Σε αυτό το άρθρο, θα σας παρουσιάσουμε τα βήματα για την εγκατάσταση του Ubuntu 24.04 με τον νέο εγκαταστάτη. Η διαδικασία είναι απλή και straightforward, ακόμη και για αρχάριους χρήστες.

## Προϋποθέσεις:

- Ένας υπολογιστής με επαρκή ισχύ
- Ένα USB flash drive (πάνω από 10gb)
- Το αρχείο ISO του Ubuntu 24.04 (μπορείτε να το κατεβάσετε από [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop){:target="\_blank"})

## Βήματα:

**1. Δημιουργήστε ένα εκκινήσιμο USB drive:**

- Χρησιμοποιήστε ένα εργαλείο όπως το [Etcher](https://etcher.balena.io/) για να δημιουργήσετε ένα εκκινήσιμο USB drive από το αρχείο ISO του Ubuntu 24.04.
- Βεβαιωθείτε ότι έχετε επιλέξει το σωστό USB drive και ότι το αρχείο ISO έχει επιλεχθεί σωστά.

**2. Επανεκκινήστε τον υπολογιστή σας και εκκινήστε από το USB drive:**

- Αλλάξτε τη σειρά εκκίνησης του υπολογιστή σας στο BIOS ή UEFI, ώστε να εκκινήσει από το USB drive.
- Η διαδικασία για την αλλαγή της σειράς εκκίνησης ενδέχεται να διαφέρει ανάλογα με τον κατασκευαστή του υπολογιστή σας. Ανατρέξτε στο εγχειρίδιο χρήσης του υπολογιστή σας ή στην ιστοσελίδα του κατασκευαστή για οδηγίες.

**3. Ξεκινήστε την εγκατάσταση του Ubuntu:**

Στο σημείο αυτό ξεκινάει ο νέος εγκαταστάτης. Πάμε να δούμε:

**3.1 Ερώτημα για την γλώσσα χρήσης του εγκαταστάτη:** Εδώ εισάγετε την γλώσσα που θέλετε να έχει ο εγκαταστάτης. Καλό είναι να το κρατήσετε στα Αγγλικά.

![Ubuntu Installer Language](/post_images/ubuntu/install/Ubuntu-24.04-installer-1.png "Ubuntu Installer Language")

**3.2 Επιλογή προσβασιμότητας:** Κάτι που έλλειπε από τον προηγούμενο εγκαταστάτη ήταν οι ρυθμίσεις για τα άτομα με περιορισμένη ακοή, όραση κλπ. Εδώ το ρυθμίζετε ανάλογα.

![Ubuntu Installer Accessibility](/post_images/ubuntu/install/Ubuntu-24.04-installer-2.png "Ubuntu Installer Accessibility")

**3.3 Επιλογή γλώσσας πληκτρολογίου:** Εδώ επιλέγετε Ελληνικά. Επειδή την έχουμε πάθει από εγκαταστάσεις και άλλων λειτουργικών, προτιμήστε να το αφήσετε Αγγλικά. Σε επόμενο βήμα όταν θα πρέπει να εισάγετε όνομα χρήστη και συνθηματικό, ίσως να σας παιδέψει εάν έχετε Ελληνικά.

![Ubuntu Installer Keyboard Language](/post_images/ubuntu/install/Ubuntu-24.04-installer-3.png "Ubuntu Installer Keyboard Language")

**3.4 Σύνδεση στο Internet:** Με τη σύνδεση στο Internet θα εγκατασταθούν πρόσθετα προγράμματα και οδηγοί που δεν βρίσκονται στο ISO. Εάν δεν διαθέτετε σύνδεση κατά την ώρα της εγκατάστασης, η διαδικασία αυτή θα γίνει μετά, με την πρώτη ενημέρωση του συστήματος.

![Ubuntu Installation Internet Connection](/post_images/ubuntu/install/Ubuntu-24.04-installer-4.png "Ubuntu Installation Internet Connection")

**3.5 Αυτόματη ή διαδραστική εγκατάσταση:** Στο σημείο αυτό μπορείτε να επιλέξετε εάν θα εγκαταστήσετε το σύστημά σας διαδραστικά (με διάφορες ερωτήσεις που θα δούμε παρακάτω) ή εάν θα γίνει αυτόματα με την χρήση του αρχείου **autoinstall.yaml**.

![Ubuntu Install Interactive or Automatic](/post_images/ubuntu/install/Ubuntu-24.04-installer-5.png "Ubuntu Install Interactive or Automatic")

**3.6 Προεπιλεγμένη ή εκτεταμένη εγκατάσταση:** Στο σημείο αυτό επιλέγετε εάν θα εγκατασταθούν ελάχιστα προγράμματα ώστε να ρυθμίσετε το σύστημα όπως θέλετε εσείς ή εάν θα εγκατασταθούν όλα τα βασικά προγράμματα όπως σουίτα γραφείου, browser κλπ.

![Ubuntu Install Default or Extended](/post_images/ubuntu/install/Ubuntu-24.04-installer-6.png "Ubuntu Install Default or Extended")

**3.7 Εγκατάσταση ιδιοταγούς λογισμικού:** Στο σημείο αυτό επιλέγετε εάν θα γίνει εγκατάσταση ιδιοταγούς λογισμικού. Αυτό κυρίως είναι κάποιοι οδηγοί για ασύρματες κάρτες αλλά και οι κωδικοποιητές για αναπαραγωγή πολυμέσων.

![Ubuntu Install Proprietary Software](/post_images/ubuntu/install/Ubuntu-24.04-installer-7.png "Ubuntu Install Proprietary Software")

Πρέπει να έχετε σύνδεση με το Internet για να μπορέσει να κατεβάσει και να εγκαταστήσει το ιδιοταγές λογισμικό.

![Ubuntu Install Proprietary Software Online](/post_images/ubuntu/install/Ubuntu-24.04-installer-8.png "Ubuntu Install Proprietary Software Online")

**3.8 Εγκατάσταση και κατατμήσεις:** Στο σημείο αυτό πρέπει να ρυθμίσετε τις κατατμήσεις για την εγκατάσταση. Εάν δεν ξέρετε τι κάνετε και δεν έχει λειτουργικό σύστημα ο υπολογιστής σας, μπορείτε να επιλέξετε τις δυο πρώτες επιλογές και κάτι θα εγκατασταθεί.  
Στην πρώτη επιλογή, εάν έχετε κάποιο λειτουργικό εγκατεστημένο, θα γίνει dual boot. Βέβαια δεν είναι 100% το αποτέλεσμα.  
Στην δεύτερη επιλογή, θα σβηστούν όλα από τον δίσκο σας και θα γίνει εγκατάσταση όπως το ρυθμίζει αυτόματα το Ubuntu.

![Ubuntu Install partitions](/post_images/ubuntu/install/Ubuntu-24.04-installer-9.png "Ubuntu Install partitions")

Η τρίτη επιλογή είναι αυτή που πρέπει να μελετήσετε γιατί είναι η πιο χρήσιμη. Θα σας εμφανίσει όλες τις κατατμήσεις.

![Ubuntu Install Partitions](/post_images/ubuntu/install/Ubuntu-24.04-installer-10.png "Ubuntu Install Partitions")

Στην παραπάνω εικόνα βλέπουμε:

- **sda1:** σύστημα αρχείων (type) VFAT. Μέγεθος 500-550ΜΒ. Θα χρησιμοποιηθεί ως κατάτμηση EFI
- **sda2:** σύστημα αρχείων (type) ext4. Μέγεθος 40-60GB. Θα χρησιμοποιηθεί για να εγκατασταθεί το λειτουργικό σύστημα (/)
- **sda3:** σύστημα αρχείων (type) ext4. Μέγεθος ότι περισέψει. Θα χρησιμοποιηθεί για αποθήκευση των αρχείων και ρυθμίσεων προγραμμάτων (/home)
- **sda4:** σύστημα αρχείων (type) swap. Μέγεθος ~8GB (παλιότερα λέγαμε διπλάσιο της φυσικής μνήμης αλλά έχει αλλάξει). Λειτουργεί ως μνήμη swap.

Στα παραπάνω μπορεί να αλλάξει το μέγεθος των κατατμήσεων (ανάλογα με τον δίσκο σας). Επίσης μπορεί να αλλάξει και ο τύπος αρχείων. πχ για το λειτουργικό σύστημα να χρησιμοποιηθεί το BtrFS και για τα προσωπικά αρχεία το σύστημα XFS.  
Στο παράδειγμά μας: **3.8.1 Κατάτμηση sda1:** ως σημείο προσάρτησης χρησιμοποιούμε το **/boot/efi** και μέγεθος είναι 550ΜΒ. Το σύστημα αρχείων γράφει VFAT αλλά μπορεί να το δείτε και ως FAT16 ή FAT32.

![Ubuntu Install Partition sda1](/post_images/ubuntu/install/Ubuntu-24.04-installer-11.png "Ubuntu Install Partition sda1")

**3.8.2 Κατάτμηση sda2:** Θα χρησιμοποιηθεί για να εγκατασταθεί το λειτουργικό σύστημα. Σύστημα αρχείων ext4 (ή BtrFS) και σημείο προσάρτησης το **/**. Το μέγεθος καλό είναι να είναι κάπως μεγάλο διότι θα εγκαταστήσετε κάποια προγράμματα flatpak ή snap και θα χρησιμοποιήσουν τον χώρο. Επίσης εάν χρειαστείτε κάποια Docker containers θα χρειαστείτε χώρο. Οπότε ανάλογα με την χρήση του, κανονίστε τι χώρο θα δώσετε.

![Ubuntu Install Partition sda2](/post_images/ubuntu/install/Ubuntu-24.04-installer-12.png "Ubuntu Install Partition sda2")

**3.8.3 Κατάτμηση sda3:** Θα χρησιμοποιηθεί για να αποθηκεύσετε τα αρχεία σας (Έγγραφα, Φωτογραφίες, Βίντεο, Μουσική κλπ). Όσο περισσότερο χώρο έχετε, τόσο το καλύτερο. Ο λόγος που χρησιμοποιούμε διαφορετική κατάτμηση είναι διότι σε περίπτωση που θέλουμε να κάνουμε εκ νέου εγκατάσταση του λειτουργικού, χωρίς να χρειαστεί να κάνουμε αντίγραφο ασφαλείας των αρχείων μας, μπορούμε να χρησιμοποιήσουμε είτε το ίδιο όνομα χρήστη με το συνθηματικό, είτε άλλο όνομα χρήστη. Στην πρώτη περίπτωση θα έχουμε κρατήσει και όλες τις ρυθμίσεις του γραφικού περιβάλλοντος ενώ στην δεύτερη περίπτωση θα πρέπει να ρυθμιστεί εκ νέου. Εδώ σύστημα αρχείων ext4 και σημείο προσάρτησης **/home**.

![Ubuntu Install Partition sda3](/post_images/ubuntu/install/Ubuntu-24.04-installer-13.png "Ubuntu Install Partition sda3")

**3.8.4 Κατάτμηση sda4:** Τέλος, η κατάτμηση για την μνήμη swap πρέπει να είναι το διπλάσιο της φυσικής μνήμης ή να είναι ~8GB.

**3.8.5 Κατάτμηση sdb1:** Μπορεί να έχετε και δεύτερο δίσκο με αρχεία. Στο **Used as:** επιλέγετε **Leave formated as Ext4**. Με αυτόν τον τρόπο δεν θα κάνει την διαμόρφωση του δίσκου (αυτή τη ρύθμιση θα χρησιμοποιήσετε και παραπάνω που αναφέραμε ότι δεν θα χρειαστεί να κάνετε αντίγραφο ασφαλείας των αρχείων σας).

![Ubuntu Install Partition sdb1](/post_images/ubuntu/install/Ubuntu-24.04-installer-14.png "Ubuntu Install Partition sdb1")

Η τελική εικόνα των κατατμήσεων του παραδείγματος μοιάζει με την παρακάτω,

![Ubuntu Installation Final Partitions](/post_images/ubuntu/install/Ubuntu-24.04-installer-15.png "Ubuntu Installation Final Partitions")

**3.9 Δημιμουργία χρήστη:** Στο σημείο αυτό δημιουργείτε έναν χρήστη εισάγοντας όλες τις πληροφορίες στα πεδία.

![Ubuntu Install User Creation](/post_images/ubuntu/install/Ubuntu-24.04-installer-16.png "Ubuntu Install User Creation")

**3.10 Επιλογή ζώνης ώρας:** Επιλέγουμε την ζώνη ώρας του υπολογιστή μας.

![Ubuntu Install Time Zone](/post_images/ubuntu/install/Ubuntu-24.04-installer-18.png "Ubuntu Install Time Zone")

**3.11 Προεπισκόπηση των ρυθμίσεων για εγκατάσταση:** Αφού ελέγξουμε ότι όλες οι ρυθμίσεις είναι σύμφωνες με αυτά που επιθυμούμε, πατάμε το κουμπί Install για εγκατάσταση.

![Ubuntu Install Review Installation](/post_images/ubuntu/install/Ubuntu-24.04-installer-19.png "Ubuntu Install Review Installation")

**3.12 Εγκατάσταση:** Αναμένουμε να ολοκληρωθεί η εγκατάσταση.

![Ubuntu Install](/post_images/ubuntu/install/Ubuntu-24.04-installer-20.png "Ubuntu Install")

![Ubuntu Install](/post_images/ubuntu/install/Ubuntu-24.04-installer-21.png "Ubuntu Install")

![Ubuntu Install](/post_images/ubuntu/install/Ubuntu-24.04-installer-22.png "Ubuntu Install")

![Ubuntu Install](/post_images/ubuntu/install/Ubuntu-24.04-installer-23.png "Ubuntu Install")

Και αφού ολοκληρωθεί, επιλέγουμε επανεκκίνηση του υπολογιστή.

![Ubuntu Install](/post_images/ubuntu/install/Ubuntu-24.04-installer-24.png "Ubuntu Install")

**4. Ολοκληρώστε την εγκατάσταση:**

**4.1 Καλοσώρισμα:** Η πρώτη οθόνη μας καλοσωρίζει στη νέα μας εγκατάσταση Ubuntu. Προσοχή, διότι το πλήκτρο **Next** βρίσκεται στο επάνω μέρος του παραθύρου. Αυτό το έχουμε δει στον εγκαταστάτη της διανομής Fedora (να έχει το πλήκτρο επόμενο στο επάνω μέρος του παραθύρου). Επίσης το συγκεκριμένο πρόγραμμα αποτελεί το καλοσώρισμα του GNOME, το οποίο χρησιμοποιούν όλες οι διανομές.

![Ubuntu welcome](/post_images/ubuntu/install/Ubuntu-24.04-installer-25.png "Ubuntu welcome")

**4.2 Εγκατάσταση Ubuntu Pro:** Στο σημείο αυτό μπορείτε να ρυθμίσετε το [Ubuntu Pro](https://eiosifidis.blogspot.com/2023/02/ubuntu-pro.html). Αυτό αφορά τις εκδόσεις LTS. Οπότε δεν γνωρίζουμε εάν εμφανίζεται σε μη LTS εκδόσεις.

![Ubuntu Pro](/post_images/ubuntu/install/Ubuntu-24.04-installer-26.png "Ubuntu Pro")

**4.3 Βοηθήστε στην βελτίωση του Ubuntu:** Εάν θέλετε να βελτιωθεί το Ubuntu συλλέγοντας κάποια δεδομένα (δεν γνωρίζουμε ποια) έτσι ώστε να έχουμε μια καλύτερη έκδοση στο μέλλον, μπορείτε να το αφήσετε ενεργό. Η αλήθεια είναι ότι δεν συλλέγει προσωπικά δεδομένα αλλά ίσως μόνο δεδομένα δυνατοτήτων του υπολογιστή σας.

![Ubuntu improve](/post_images/ubuntu/install/Ubuntu-24.04-installer-27.png "Ubuntu improve")

4.4 Εγκατάσταση περισσότερων εφαρμογών: Πριν τελειώσετε το καλοσώρισμα, μπορείτε να ανοίξετε το App Center και να εγκαταστήσετε τις εφαρμογές που επιθυμείτε. Σε αντίθετη περίπτωση πατήστε Finish και μπορείτε να χρησιμοποιήσετε τη νέα εγκατάσταση Ubuntu.

![Ubuntu App Center](/post_images/ubuntu/install/Ubuntu-24.04-installer-28.png "Ubuntu App Center")

## Συμβουλές

Εάν αντιμετωπίσετε προβλήματα κατά την εγκατάσταση, μπορείτε να ανατρέξετε στην επίσημη τεκμηρίωση του Ubuntu [https://help.ubuntu.com/lts/ubuntu-help/index.html](https://help.ubuntu.com/lts/ubuntu-help/index.html){:target="\_blank"} ή να αναζητήσετε βοήθεια σε [φόρουμ](https://forum.ubuntu-gr.org/) της κοινότητας Ubuntu.  
Μπορείτε επίσης να εγκαταστήσετε το Ubuntu 24.04 σε μια εικονική μηχανή, εάν θέλετε να το δοκιμάσετε πριν το εγκαταστήσετε στον κύριο υπολογιστή σας.

Εάν βρήκατε χρήσιμο το άρθρο, μπορείτε να το μοιραστείτε με φίλους σας που θέλουν να εγκαταστήσουν Ubuntu με το νέο εγκαταστάτη.

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2024/06/ubuntu-22.04-install.html](https://eiosifidis.blogspot.com/2024/06/ubuntu-22.04-install.html){:target="\_blank"}
