---
layout: post
title: "Μετακίνηση του Docker storage directory από το root στο /home"
date: 2025-02-08 12:00:00
description: Μάθετε πώς να μετακινήσετε το Docker storage directory από το root στο /home για καλύτερη διαχείριση χώρου, αποφυγή γεμάτου partition.
tags:
- docker
- docker storage directory
categories:
- Greek
- DOCKER
twitter_text: 'Μετακίνηση του Docker storage directory από το root στο /home'
---

![Docker storage directory από το root στο /home](/post_images/misc/docker-move-containers-truck.png "Docker storage directory από το root στο /home"){:width="320px"}

Η μετακίνηση του Docker storage directory από τον προεπιλεγμένο κατάλογο **/var/lib/docker** σε έναν νέο κατάλογο στο /home μπορεί να αποτελέσει μια σημαντική απόφαση για πολλούς χρήστες, ιδιαίτερα όταν το root partition έχει περιορισμένο χώρο ή όταν θέλουμε να διαχειριζόμαστε πιο αποτελεσματικά τα δεδομένα και τις εικόνες των containers. Αυτή η αλλαγή βοηθάει στην αποφυγή γεμάτου root partition, διευκολύνει τη δημιουργία backups και επιτρέπει την οργάνωση των Docker δεδομένων σε έναν ξεχωριστό δίσκο ή partition με μεγαλύτερη χωρητικότητα. Επιπλέον, προσφέρει μεγαλύτερη ευελιξία στη διαχείριση του χώρου και την αποφυγή προβλημάτων απόδοσης λόγω έλλειψης χώρου. Σε αυτό το άρθρο, θα εξετάσουμε βήμα προς βήμα πώς να πραγματοποιήσετε αυτήν τη μετακίνηση και τους λόγους για τους οποίους μπορεί να είναι απαραίτητη.  
  
Για να αποθηκεύεις τα Docker containers στο home directory σου αντί για το root partition, πρέπει να αλλάξεις το Docker storage directory. Μπορείς να το κάνεις ως εξής: 

1. Δες την τρέχουσα τοποθεσία αποθήκευσης:

{% highlight ruby %}
docker info | grep "Docker Root Dir"
{% endhighlight %}

Συνήθως είναι στο **/var/lib/docker**, που βρίσκεται στο root partition.  
  
2. Μετακίνησε το **Docker storage** στο home directory σου. Αν π.χ. θέλεις να το βάλεις στο **~/docker-data**, εκτέλεσε:

{% highlight ruby %}
sudo systemctl stop docker  
sudo mkdir -p ~/docker-data  
sudo mv /var/lib/docker ~/docker-data/
{% endhighlight %}

3. Αλλαγή διαμόρφωσης του Docker:  
  
Άνοιξε ή δημιούργησε το αρχείο /etc/docker/daemon.json. Το αρχείο daemon.json μπορεί να μην υπάρχει από προεπιλογή, αλλά μπορείς να το δημιουργήσεις εσύ. Στις περισσότερες διανομές, το σωστό path είναι:

{% highlight ruby %}
/etc/docker/daemon.json
{% endhighlight %}

Αν δεν υπάρχει, μπορείς να το δημιουργήσεις με:

{% highlight ruby %}
sudo touch /etc/docker/daemon.json
{% endhighlight %}

Έπειτα, άνοιξέ το με έναν επεξεργαστή κειμένου, π.χ.:

{% highlight ruby %}
sudo nano /etc/docker/daemon.json
{% endhighlight %}

και πρόσθεσε:

{% highlight ruby %}
{
  "data-root": "/home/your-username/docker-data"
}
{% endhighlight %}

(Αντικατέστησε το your-username με το πραγματικό username σου.)  
  
Αν πάλι δεν υπάρχει ο κατάλογος /etc/docker/, μπορείς να το δημιουργήσεις με:

{% highlight ruby %}
sudo mkdir -p /etc/docker
{% endhighlight %}

και μετά να δημιουργήσεις το daemon.json, όπως παραπάνω.  
  
4. Επανεκκίνηση του Docker:

{% highlight ruby %}
sudo systemctl daemon-reload  
sudo systemctl start docker
{% endhighlight %}

5. Επιβεβαίωση ότι η αλλαγή εφαρμόστηκε:

{% highlight ruby %}
docker info | grep "Docker Root Dir"
{% endhighlight %}

Αν όλα είναι σωστά, το νέο path θα εμφανίζεται στο output.  
  
Τώρα τα containers και images θα αποθηκεύονται στο home directory σου! 🚀

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2025/02/docker-storage-directory-root-home.html](https://eiosifidis.blogspot.com/2025/02/docker-storage-directory-root-home.html){:target="_blank"}
