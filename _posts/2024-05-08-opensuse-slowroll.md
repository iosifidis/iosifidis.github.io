---
layout: post
title: "Τι είναι η openSUSE Slowroll; Πως να την εγκαταστήσω;"
date: 2024-05-08 12:00:00
description: Μια πιο αργή έκδοση Tumbleweed
tags:
  - opensuse
  - slowroll
categories:
  - Greek
  - openSUSE
twitter_text: "Τι είναι η openSUSE Slowroll; Πως να την εγκαταστήσω;"
---

![Slowroll Logo](/post_images/opensuse/slowroll/Slowroll_logo_by_pprmint_text.png "Slowroll Logo"){:width="320px"}

# Τι είναι;

Η Slowroll είναι μια νέα διανομή από το 2023 που βασίζεται στο Tumbleweed, αλλά "κυλάει" πιο αργά. Με μεγάλες ενημερώσεις κάθε έναν ή δύο μήνες, και συνεχείς διορθώσεις σφαλμάτων και διορθώσεις ασφαλείας καθώς εμφανίζονται.

![Slowroll vs Tumbleweed](/post_images/opensuse/slowroll/Slowroll-vs-tumbleweed-updates.png "Slowroll  vs Tumbleweed"){:width="320px"}

# Εγκατάσταση - Χρήση

Για αρχική εγκατάσταση, μπορείτε να χρησιμοποιήσετε το DVD iso από τη διεύθυνση [http://download.opensuse.org/slowroll/iso/](http://download.opensuse.org/slowroll/iso/){:target="\_blank"} αλλά να αφήσετε τα διαδικτυακά αποθετήρια απενεργοποιημένα (έτσι δεν τραβάει νεότερα πακέτα Tumbleweed από online repos). Μπορείτε επίσης να μεταβείτε απευθείας από οποιαδήποτε κυκλοφορία Leap ή Tumbleweed στο Slowroll αντικαθιστώντας τα αποθετήρια.

Μετά την εγκατάσταση από DVD, πρέπει να αντικαταστήσετε το Tumbleweed με τα αποθετήρια Slowroll. Το ίδιο ισχύει όταν αλλάζετε από το Leap ή ένα παλαιότερο στιγμιότυπο του Tumbleweed στο Slowroll.

{% highlight ruby %}
zypper in openSUSE-repos-Slowroll -openSUSE-repos-Tumbleweed
{% endhighlight %}

Δεν συνιστούμε τη χρήση αποθετηρίων ανάπτυξης, εκτός εάν αυτά έχουν μεταγλωττιστεί ειδικά για το Slowroll. Τα αποθετήρια τρίτων που δεν έχουν δοκιμαστεί με το Tumbleweed ενδέχεται να χαλάσουν την εγκατάστασή σας.

Το Packman μπορεί να λειτουργεί, αλλά μπορεί επίσης να χαλάει το σύστημα περιστασιακά. Υπάρχει ένα ειδικό αποθετήριο packman για το Slowroll:

{% highlight ruby %}
zypper ar --refresh -p 70 http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Slowroll/Essentials/ packman
{% endhighlight %}

Όπως το Tumbleweed, χρησιμοποιήστε το zypper dup για αναβάθμιση.

Πηγές  
[Reddit](https://www.reddit.com/r/openSUSE_Slowroll/){:target="\_blank"}  
[Forums.o.o](https://forums.opensuse.org/tag/slowroll){:target="\_blank"}  
[Mailing-list](https://lists.opensuse.org/archives/search?mlist=factory%40lists.opensuse.org&q=Slowroll){:target="\_blank"}  
[Wiki](https://en.opensuse.org/openSUSE:Slowroll){:target="\_blank"}  
Bugzilla TBD

# Ανάπτυξη

O bmwiedemann έκανε το σχεδιασμό και το script .

Η ανάπτυξη γίνεται στο [https://build.opensuse.org/project/show/openSUSE:Slowroll](https://build.opensuse.org/project/show/openSUSE:Slowroll){:target="\_blank"} με τη χρήση του [https://github.com/bmwiedemann/slowroll-tools](https://github.com/bmwiedemann/slowroll-tools)

Τα μη δοκιμασμένα πακέτα μπαίνουν στη διεύθυνση [https://build.opensuse.org/project/show/openSUSE:Slowroll:Staging](https://build.opensuse.org/project/show/openSUSE:Slowroll:Staging){:target="\_blank"} πρώτα και ελέγχονται από το openQA (TBD)

Οι περισσότερες ενημερώσεις θα πρέπει να υποβάλλονται στο Factory και θα μετεγκατασταθούν αυτόματα στο Slowroll μετά την αποδοχή. Φροντίστε να αναφέρετε σχετικές επιδιορθώσεις CVE και αναφορές boo# σε αρχεία .changes για να επιταχύνετε τη μετεγκατάσταση. Οι άμεσες υποβολές θα πρέπει να χρειάζονται μόνο για backport επειγουσών επιδιορθώσεων που απαιτούν ενημερωμένα βασικά πακέτα στο Factory (τα οποία είναι πολύ επικίνδυνα για γρήγορη ενημέρωση)

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2024/04/opensuse-slowroll.html](https://eiosifidis.blogspot.com/2024/04/opensuse-slowroll.html){:target="\_blank"}
