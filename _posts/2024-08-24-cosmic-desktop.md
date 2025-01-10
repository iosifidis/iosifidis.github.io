---
layout: post
title: "Εξερευνώντας το Cosmic Desktop: Μια λεπτομερής πρώτη ματιά"
date: 2024-08-24 10:00:00
description: Ανακαλύψτε το νέο Cosmic Desktop από τη System76. Δείτε τις δυνατότητες του και την εξέλιξη του Pop!_OS.
tags:
- COSMIC
- Pop!_OS
- System76
- GUI
categories:
- Greek
- COSMIC
- Pop!_OS
- System76
- GUI
twitter_text: 'Εξερευνώντας το Cosmic Desktop: Μια λεπτομερής πρώτη ματιά'
---

![COSMIC Logo](/post_images/cosmic/COSMIC-logo-header.png "COSMIC Logo")

Το [Cosmic desktop](https://github.com/pop-os/cosmic-epoch/?ref=iosifidis.gr) (cosmic-epoch) είναι ένα νέο περιβάλλον επιφάνειας εργασίας που αναπτύσσεται από τον κατασκευαστή υπολογιστών [System76](https://system76.com/?ref=iosifidis.gr) για τη διανομή Linux [Pop!\_OS](https://pop.system76.com/?ref=iosifidis.gr). Είναι υπό ανάπτυξη εδώ και αρκετό καιρό και γράφεται από την αρχή. Η επιφάνεια εργασίας [Cosmic](https://system76.com/cosmic) αναπτύχθηκε κυρίως χρησιμοποιώντας τη [Rust](https://www.rust-lang.org/?ref=iosifidis.gr) ως κύρια γλώσσα, με δημοφιλείς βιβλιοθήκες όπως η ice και η libcosmic. Ο κύριος κινητήριος παράγοντας για το System76 είναι να απομακρυνθεί από το GNOME που βασίζεται στο GTK για το οικοσύστημα εφαρμογών και λογισμικού του.  
  
Πολλοί χρήστες θαυμάζουν την προσέγγιση που ακολουθεί η System76 για την εφαρμογή λειτουργιών φιλικών προς τον χρήστη στο Pop!\_OS, όπως η τοποθέτηση πλακιδίων παραθύρων. Η εισαγωγή ενός νέου περιβάλλοντος επιφάνειας εργασίας τους επιτρέπει να κάνουν περισσότερα τέτοια πράγματα με καλύτερο τρόπο.  
  
## Επιφάνεια εργασίας Cosmic: Με μια ματιά

### Αξίζει την αναμονή;

Όταν προσπαθείτε για πρώτη φορά να συνδεθείτε στην επιφάνεια εργασίας Cosmic, θα πρέπει να δείτε την συνερδία Cosmic στην οθόνη σύνδεσης οποιασδήποτε διανομής όπου την έχετε εγκαταστήσει. Ένα από τα πράγματα που πρέπει να προσέξετε είναι ότι **η συνεδρία Cosmic στην επιφάνεια εργασίας έρχεται μόνο με το Wayland**.  
  
Παρόλο που το COSMIC είναι μόνο για το Wayland, έχει υποστήριξη για εφαρμογές X11 μέσω του XWayland, επομένως δεν χρειάζεται να ανησυχείτε μήπως οι αγαπημένες σας εφαρμογές δεν λειτουργούν σωστά.

![COSMIC Desktop](/post_images/cosmic/proti-matia/COSMIC_a-1.jpg "COSMIC Desktop")

Προχωρώντας σε πράγματα που το κάνουν να αξίζει την αναμονή. Εδώ είναι τα βασικά χαρακτηριστικά του COSMIC:

*   Θέμα εφαρμογών   
*   COSMIC Αρχεία   
*   COSMIC Επεξεργασία   
*   COSMIC Τερματικό   
*   COSMIC App Store   
*   COSMIC Εργαλείο Λήψης Στιγμιοτύπων   
*   Σύγχρονη οθόνη κλειδώματος/σύνδεσης   
*   Προηγμένη διαχείριση παραθύρων   

### Θέμα εφαρμογών

![Θέμα εφαρμογών](/post_images/cosmic/proti-matia/COSMIC_b.jpg "Θέμα εφαρμογών")

Η διεπαφή COSMIC διαθέτει υποστήριξη θεμάτων για εφαρμογές GTK 3/4 και Flatpak, με προσαρμοσμένα θέματα να εφαρμόζονται επίσης σε εφαρμογές GTK εάν είναι ενεργοποιημένο ένα καθολικό θέμα. Υπάρχει επίσης η δυνατότητα προσθήκης προσαρμοσμένων θεμάτων εικονιδίων για εφαρμογές COSMIC και GTK.  
  
Καθώς βρισκόμαστε στο θέμα των εφαρμογών, η διεπαφή COSMIC συνοδεύεται από πολλές εγγενείς εφαρμογές που βασίζονται στην Rust, οι οποίες έχουν σχεδιαστεί ειδικά για να ταιριάζουν καλά με το περιβάλλον επιφάνειας εργασίας, διατηρώντας παράλληλα υπόψη την προσβασιμότητα.  
  
### COSMIC Αρχεία

![COSMIC Αρχεία](/post_images/cosmic/proti-matia/COSMIC_c.jpg "COSMIC Αρχεία")

Όχι, δεν είναι ο διαχειριστής αρχείων nautilus. Είναι το **COSMIC Files** (COSMIC Αρχεία), το οποίο είναι από τις νεότερες προσθήκες. Φυσικά, βλέπετε μια ομοιότητα, φροντίζοντας να μπορείτε να πλοηγηθείτε στα αρχεία/φακέλους σας με τρόπο που ήδη γνωρίζετε.  
  

### COSMIC Επεξεργαστής

![COSMIC Επεξεργαστής](/post_images/cosmic/proti-matia/COSMIC_f.jpg "COSMIC Επεξεργαστής")

Στη συνέχεια, έρχεται το **COSMIC Edit** (COSMIC Επεξεργαστής), το οποίο είναι ένα ελαφρύ πρόγραμμα επεξεργασίας κειμένου που υποστηρίζει αμφίδρομο κείμενο (ανάγνωση από αριστερά προς δεξιά/δεξιά προς τα αριστερά), συνδέσμους, emoji και πολλά άλλα.  
  
Έρχεται επίσης με πρόσθετες λειτουργίες όπως επεξεργασία σε στυλ Vim, ενσωμάτωση Git, δυνατότητα αναζήτησης έργων, εμφάνιση του αριθμού λέξεων και ακόμη και επισήμανση της τρέχουσας γραμμής.  
  
Δεν νομίζω ότι χρειαζόμασταν ένα πρόγραμμα επεξεργασίας κειμένου ειδικά για το COSMIC, αλλά αν δικαιολογεί την ύπαρξή του, πολλοί χρήστες που αναζητούν ένα ελαφρύ και ισχυρό πρόγραμμα επεξεργασίας θα ήταν ευχαριστημένοι.  
  
### COSMIC Τερματικό

![COSMIC Τερματικό](/post_images/cosmic/proti-matia/COSMIC_d.jpg "COSMIC Τερματικό")

Όπως υποδηλώνει το όνομα, το COSMIC Terminal είναι ο εξομοιωτής τερματικού για το COSMIC, το οποίο δημιουργήθηκε χρησιμοποιώντας το πλαίσιο [alacritty\_terminal](https://crates.io/crates/alacritty_terminal?ref=iosifidis.gr) και μια προσαρμοσμένη απόδοση που βασίζεται στο [COSMIC Text](https://github.com/pop-os/cosmic-text?ref=iosifidis.gr).  
  
Έχει μερικά πραγματικά προσεγμένα χαρακτηριστικά, όπως υποστήριξη για κείμενο [LTR](https://developer.mozilla.org/en-US/docs/Glossary/LTR?ref=iosifidis.gr) και [RTL](https://en.wikipedia.org/wiki/Right-to-left_script?ref=iosifidis.gr), θέματα επιφάνειας εργασίας, θέματα σύνταξης, απόδοση GPU κ.λπ.  
  
Υπάρχει επίσης κάτι που ονομάζεται splits, το οποίο επιτρέπει στο τερματικό να χωρίζεται κατακόρυφα, παρέχοντας πρόσβαση σε δύο περιοχές εργασίας, με την επιλογή πολλαπλών διαχωρισμών.  
  
### COSMIC App Store

![COSMIC App Store](/post_images/cosmic/proti-matia/COSMIC_e.jpg "COSMIC App Store")

Υπάρχει το COSMIC App Store, το οποίο είναι μοντέρνο και έχει μια οικεία διάταξη με εφαρμογές που είναι τακτοποιημένες σε κατηγορίες. Επιπλέον, υπάρχει επίσης μια εύχρηστη καρτέλα "Ενημερώσεις" που σας ειδοποιεί όταν υπάρχει μια νέα εφαρμογή ή αναβάθμιση συστήματος.  

### COSMIC Εργαλείο Λήψης Στιγμιοτύπων

![COSMIC Εργαλείο Λήψης Στιγμιοτύπων](/post_images/cosmic/proti-matia/COSMIC_e.jpg "COSMIC Εργαλείο Λήψης Στιγμιοτύπων")

Το COSMIC διαθέτει επίσης ένα εγγενές εργαλείο στιγμιότυπων οθόνης που σας επιτρέπει να τραβάτε στιγμιότυπα οθόνης ολόκληρης της οθόνης, μιας συγκεκριμένης εφαρμογής ή μιας επιλεγμένης περιοχής. Μπορεί να το βρείτε λίγο παρόμοιο με αυτό που έχει το GNOME, αλλά με μερικές ακόμη επιλογές.  
  
### Σύγχρονη οθόνη κλειδώματος/σύνδεσης

![COSMIC Σύγχρονη οθόνη κλειδώματος/σύνδεσης](/post_images/cosmic/proti-matia/COSMIC_h.jpg "COSMIC Σύγχρονη οθόνη κλειδώματος/σύνδεσης")

Η εμπειρία σύνδεσης στο COSMIC πρόκειται να είναι κάτι μοναδικό. Όπως μπορείτε να δείτε από το αρχικό μοντέλο σχεδίασης παραπάνω, η οθόνη σύνδεσης/κλειδώματος ακολουθεί μια κινητή προσέγγιση, εμφανίζει πληροφορίες και παρέχει πρόσβαση σε αυτές σε έναν οργανωμένο χώρο, αντί να έχει πράγματα γύρω από την οθόνη.  
  
### Προηγμένη διαχείριση παραθύρων

![COSMIC Προηγμένη διαχείριση παραθύρων](/post_images/cosmic/proti-matia/COSMIC_i-1.jpg "COSMIC Προηγμένη διαχείριση παραθύρων")

Ενώ το Pop!\_OS είχε ήδη την καλύτερη εφαρμογή για τοποθέτηση πλακιδίων παραθύρων, το COSMIC το κάνει πιο απλό δίνοντάς του περισσότερο έλεγχο.  
  
Η δυνατότητα προσαρμογής της συμπεριφοράς του χώρου εργασίας με μια γρήγορη επιλογή και άλλες συνήθεις συντομεύσεις πληκτρολογίου που γνωρίζουμε στο Pop!\_OS.

![COSMIC](/post_images/cosmic/proti-matia/COSMIC_j-1.jpg "COSMIC")

Η νέα μικροεφαρμογή πλακιδίων που μπορεί να χρησιμοποιηθεί για την προσαρμογή της συμπεριφοράς τοποθέτησης πλακιδίων παραθύρων, με επιλογές για την ενεργοποίηση της αυτόματης τοποθέτησης πλακιδίων των παραθύρων και περαιτέρω υποεπιλογές για εναλλαγή της αυτόματης τοποθέτησης πλακιδίων ανά χώρο εργασίας και για αλλαγή της νέας συμπεριφοράς του χώρου εργασίας.

![COSMIC](/post_images/cosmic/proti-matia/COSMIC_k.jpg "COSMIC")

Έχετε επίσης περισσότερες επιλογές για τη διαχείριση των παραθύρων. απλά κάντε δεξί κλικ στην κεφαλίδα (επάνω μέρος) ενός παραθύρου και θα λάβετε επιλογές όπως μετακίνηση, αλλαγή μεγέθους, μεγιστοποίηση, αιώρηση, λήψη στιγμιότυπου οθόνης κ.λπ.  
 
## Πότε είναι η ημερομηνία κυκλοφορίας;

Η επιφάνεια εργασίας COSMIC έκανε το ντεμπούτο της με την έκδοση **Pop!\_OS 24.04 LTS Alpha**. Είναι μια ασταθής έκδοση με χαρακτηριστικά που λείπουν. Αλλά, μπορείτε να πειραματιστείτε με αυτό αν θέλετε.  
  
Θα ακολουθήσουν τρόποι εγκατάστασης σε σε άλλες διανομές. Περισσότερα στην επίσημη [ιστοσελίδα COSMIC](https://system76.com/cosmic?ref=iosifidis.gr).

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2024/08/cosmic-desktop.html](https://eiosifidis.blogspot.com/2024/08/cosmic-desktop.html){:target="_blank"}