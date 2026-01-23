---
layout: post
title: "Πώς να επαναφέρετε το συνθηματικό Root σε διανομές Linux"
date: 2026-01-23 12:00:00
description: Ξέχασες το συνθηματικό του root; Πλήρης οδηγός επαναφοράς για Debian, Ubuntu, Mint, Arch, Fedora, openSUSE. Βήμα-βήμα οδηγίες.
tags:
  - restore
  - root
  - password
  - devops
categories:
  - Greek
  - ROOT
  - PASSWORD
  - DEVOPS
twitter_text: "Πώς να επαναφέρετε το συνθηματικό Root σε διανομές Linux"
---

![Πώς να επαναφέρετε το συνθηματικό Root σε διανομές Linux](/post_images/devops/restore-password-root.webp "Πώς να επαναφέρετε το συνθηματικό Root σε διανομές Linux"){:width="500px"}

Πλήρης οδηγός για Debian, Ubuntu, Linux Mint, Arch, Fedora και openSUSE

Εισαγωγή
--------

Κατά καιρούς έχω βοηθήσει χρήστες να εγκαταστήσουν μια διανομή Linux, είτε το έχω εγκαταστήσει εγώ γι'αυτούς. Η πρώτη κίνηση που κάνω πάντα είναι να τους ρωτάω το συνθηματικό που θέλουν να χρησιμοποιηθεί, ώστε να τον θυμούνται πάντα. Αν τυχόν δεν μπορούν να μου πουν εκείνη την στιγμή, βάζω κάτι εύκολο για να κάνω την δουλειά μου και στο τέλος αλλάζουμε το συνθηματικό σε αυτό που θα θυμούνται.

⚠️ Σημαντική Συμβουλή: Αν τυχόν δεν θυμάσαι το συνθηματικό, γράψε το κάπου όπου θα είναι ασφαλές, γιατί εάν δεν τον θυμάσαι θα κλειδωθείς εκτός συστήματος!

Ναι, το ξέρω, το να το γράψεις κάπου δεν είναι και ότι καλύτερο από θέματα ασφαλείας, αλλά εάν δεν το θυμάσαι, η επόμενη κίνηση είναι να εγκαταστήσουμε ξανά το λειτουργικό - πράγμα που δεν είναι ότι καλύτερο.Υπάρχει βέβαια και η επαναφορά που θα δούμε εδώ, αλλά μην στηρίζεστε σε αυτό. Πάμε να δούμε αναλυτικά:

1️⃣ Debian
----------

### Βήμα 1: Πρόσβαση στο GRUB

Στο μενού GRUB, στην πρώτη επιλογή εκκίνησης, πατήστε το πλήκτρο **e**. Αυτό θα σας δώσει τη δυνατότητα να επεξεργαστείτε τις παραμέτρους εκκίνησης.

### Βήμα 2: Τροποποίηση παραμέτρων

Βρείτε την γραμμή που αρχίζει με την λέξη **linux** και εντοπίστε τις παραμέτρους του `/boot/vmlinuz-*`.

Μετακινηθείτε στο τέλος της γραμμής, μετά το `ro quiet` και προσθέστε:

{% highlight ruby %}
init=/bin/bash
{% endhighlight %}

Πατήστε **Ctrl+X** για να εκκινήσετε.

### Βήμα 3: Remount με δικαιώματα εγγραφής

Το σύστημα αρχείων θα είναι προσαρτημένο μόνο για ανάγνωση. Εκτελέστε:

{% highlight ruby %}
mount -n -o remount,rw /
{% endhighlight %}

### Βήμα 4: Αλλαγή συνθηματικού

Αλλάξτε το συνθηματικό του root:

{% highlight ruby %}
passwd
{% endhighlight %}

Για άλλο χρήστη χρησιμοποιήστε: `passwd username`

Θα δείτε το μήνυμα: "password updated successfully"

### Βήμα 5: Επανεκκίνηση

Γράψτε **sync** και πατήστε **Ctrl + Alt + Del** για επανεκκίνηση.

[📸 Δείτε screenshots εδώ →](https://www.tecmint.com/reset-forgotten-root-password-in-debian/)

2️⃣ Linux Mint / Ubuntu
-----------------------

### Βήμα 1: Πρόσβαση στο GRUB

Στο μενού GRUB, πατήστε το πλήκτρο **e** για επεξεργασία.

### Βήμα 2: Τροποποίηση παραμέτρων

Βρείτε την γραμμή που αρχίζει με **linux** και εντοπίστε το `/vmlinuz-*`.

Μετακινηθείτε στο τέλος της γραμμής, μετά το `ro quiet splash` και προσθέστε:

{% highlight ruby %}
rw init=/bin/bash
{% endhighlight %}

Πατήστε **Ctrl+X** ή **F10** για εκκίνηση.

### Βήμα 3: Επαλήθευση δικαιωμάτων

Μπορείτε να επαληθεύσετε ότι έχετε δικαιώματα εγγραφής:

{% highlight ruby %}
mount | grep -w /
{% endhighlight %}

### Βήμα 4: Αλλαγή συνθηματικού

Αλλάξτε το συνθηματικό:

{% highlight ruby %}
passwd root
{% endhighlight %}

Ή για άλλο χρήστη: `passwd username`

**💡 Σημείωση:** Στο Ubuntu και Linux Mint, ο λογαριασμός root είναι απενεργοποιημένος εξ ορισμού. Αν θέλετε να αλλάξετε το συνθηματικό του κανονικού χρήστη σας, χρησιμοποιήστε `passwd your_username`

### Βήμα 5: Επανεκκίνηση

Γράψτε **sync**. Επανεκκινήστε με:

{% highlight ruby %}
exec /sbin/init
{% endhighlight %}

ή **Ctrl + Alt + Del**

[📸 Screenshots για Linux Mint →](https://www.tecmint.com/reset-forgotten-root-password-in-linux-mint/)  
[📸 Screenshots για Ubuntu →](https://www.tecmint.com/reset-forgotten-root-password-in-ubuntu/)

3️⃣ Fedora / RHEL / CentOS / Rocky Linux
----------------------------------------

### Βήμα 1: Επεξεργασία GRUB

Στο μενού GRUB, πατήστε **e**.

### Βήμα 2: Προσθήκη παραμέτρων

Βρείτε την γραμμή που αρχίζει με **linux** και το `/vmlinuz-*`.

Στο τέλος της γραμμής, μετά το `rhgb quiet`, **ΑΝΤΙΚΑΤΑΣΤΗΣΤΕ** με:

{% highlight ruby %}
rd.break enforcing=0
{% endhighlight %}

Πατήστε **Ctrl+X** ή **F10**.

### Βήμα 3: Remount του Sysroot

{% highlight ruby %}
mount -o remount,rw /sysroot
{% endhighlight %}

### Βήμα 4: Chroot στο σύστημα

{% highlight ruby %}
chroot /sysroot
{% endhighlight %}

### Βήμα 5: Αλλαγή συνθηματικού

{% highlight ruby %}
passwd
{% endhighlight %}

Για άλλο χρήστη: `passwd username`

### Βήμα 6: Επανεκκίνηση

Γράψτε **sync** και πατήστε **Ctrl + Alt + Del** για επανεκκίνηση.

### Βήμα 7: Επαναφορά SELinux

Μετά την επανεκκίνηση, συνδεθείτε ως root και εκτελέστε:

{% highlight ruby %}
restorecon -v /etc/shadow   setenforce 1
{% endhighlight %}

**💡 Σημείωση:** Η επαναφορά του SELinux είναι κρίσιμη σε συστήματα που το χρησιμοποιούν, διαφορετικά μπορεί να αντιμετωπίσετε προβλήματα σύνδεσης.

[📸 Δείτε screenshots εδώ →](https://www.tecmint.com/reset-forgotten-root-password-in-fedora/)

4️⃣ openSUSE / SUSE Linux Enterprise
------------------------------------

### Μέθοδος 1: Χρήση init=/bin/bash

#### Βήμα 1: Επεξεργασία GRUB

Στο μενού GRUB, πατήστε **e** για επεξεργασία.

#### Βήμα 2: Τροποποίηση παραμέτρων

Βρείτε την γραμμή που αρχίζει με **linux** και εντοπίστε το `/boot/vmlinuz-*`.

Στο τέλος της γραμμής, μετά το `splash=silent quiet` (ή `quiet`), προσθέστε:

{% highlight ruby %}
init=/bin/bash
{% endhighlight %}

Πατήστε **Ctrl+X** ή **F10** για εκκίνηση.

#### Βήμα 3: Remount με δικαιώματα εγγραφής

{% highlight ruby %}
mount -o remount,rw /
{% endhighlight %}

#### Βήμα 4: Αλλαγή συνθηματικού

{% highlight ruby %}
passwd
{% endhighlight %}

Για άλλο χρήστη: `passwd username`

#### Βήμα 5: Επανεκκίνηση

{% highlight ruby %}
exec /sbin/init
{% endhighlight %}

ή γράψτε **sync** και πατήστε **Ctrl + Alt + Del**.

### Μέθοδος 2: Χρήση Single User Mode (Εναλλακτική)

#### Βήμα 1: Επεξεργασία GRUB

Στο μενού GRUB, πατήστε **e**.

#### Βήμα 2: Προσθήκη παραμέτρου

Στο τέλος της γραμμής **linux**, προσθέστε:

{% highlight ruby %}
systemd.unit=rescue.target
{% endhighlight %}

Πατήστε **Ctrl+X** για εκκίνηση.

#### Βήμα 3: Σύνδεση και αλλαγή συνθηματικού

Θα σας ζητηθεί να εισάγετε το τρέχον συνθηματικό του root (ή Enter αν δεν υπάρχει). Στη συνέχεια:

{% highlight ruby %}
passwd   reboot
{% endhighlight %}

**💡 Σημείωση για openSUSE:** Το openSUSE σε παλιότερες εκδόσεις της Leap 16.0, χρησιμοποιεί το YaST για διαχείριση συστήματος. Εναλλακτικά, μπορείτε να εκκινήσετε από το εγκαταστάτη και να χρησιμοποιήσετε το "Rescue System" για να αλλάξετε το συνθηματικό.

5️⃣ Arch Linux
--------------

### Βήμα 1: Επεξεργασία GRUB

Στο μενού GRUB, πατήστε **e** για επεξεργασία.

### Βήμα 2: Προσθήκη παραμέτρων

Βρείτε την γραμμή που αρχίζει με **linux** και το `/boot/vmlinuz-linux`.

Στο τέλος της γραμμής, μετά το `quiet`, προσθέστε:

{% highlight ruby %}
init=/bin/bash
{% endhighlight %}

Πατήστε **Ctrl+X** ή **F10**.

### Βήμα 3: Remount με δικαιώματα εγγραφής

{% highlight ruby %}
mount -n -o remount,rw /
{% endhighlight %}

### Βήμα 4: Αλλαγή συνθηματικού

{% highlight ruby %}
passwd
{% endhighlight %}

Για άλλο χρήστη: `passwd username`

### Βήμα 5: Επανεκκίνηση

{% highlight ruby %}
exec /sbin/init
{% endhighlight %}

[📸 Δείτε screenshots εδώ →](https://www.tecmint.com/reset-forgotten-root-password-in-arch-linux/)

⚠️ Σημαντικές Συμβουλές Ασφαλείας
---------------------------------

*   **Φυσική πρόσβαση:** Οι παραπάνω μέθοδοι λειτουργούν μόνο με φυσική πρόσβαση στον υπολογιστή. Αυτό δείχνει γιατί είναι σημαντικό να προστατεύετε φυσικά τους servers σας.
*   **GRUB Password:** Για επιπλέον ασφάλεια, μπορείτε να θέσετε συνθηματικό στο GRUB για να αποτρέψετε την επεξεργασία παραμέτρων εκκίνησης.
*   **Κρυπτογράφηση δίσκου:** Η πλήρης κρυπτογράφηση δίσκου (LUKS) προσθέτει ένα επιπλέον επίπεδο προστασίας.
*   **Διαχείριση συνθηματικών:** Χρησιμοποιήστε έναν password manager για να αποθηκεύετε με ασφάλεια τα συνθηματικά σας.
*   **Δοκιμή:** Πριν εφαρμόσετε αυτές τις μεθόδους σε production συστήματα, δοκιμάστε τις σε ένα test περιβάλλον.

Συμπέρασμα
----------

Η επαναφορά συνθηματικού root στο Linux είναι μια σχετικά απλή διαδικασία που ποικίλλει ελάχιστα ανάμεσα στις διαφορετικές διανομές. Το σημαντικότερο είναι να θυμάστε πάντα τα συνθηματικά σας ή να τα αποθηκεύετε με ασφάλεια. Αν και αυτές οι μέθοδοι μπορούν να σας σώσουν από μια δύσκολη κατάσταση, είναι προτιμότερο να μην χρειαστεί ποτέ να τις χρησιμοποιήσετε.

Αν αυτό το άρθρο σας βοήθησε, κάντε share σε φίλους που χρησιμοποιούν Linux!

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2026/01/epanafora-pasword-root.html](https://eiosifidis.blogspot.com/2026/01/epanafora-pasword-root.html){:target="_blank"}
