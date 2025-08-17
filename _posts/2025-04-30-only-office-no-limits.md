---
layout: post
title: "Δημιουργία ONLYOFFICE χωρίς το όριο των 20 συνδέσεων"
date: 2025-04-30 12:00:00
description: Οδηγός build για νεότερες, απεριόριστες εκδόσεις ONLYOFFICE. Φτιάξτε το δικό σας .deb από source με GitHub, Docker ή Actions. Χωρίς όρια σύνδεσης!
tags:
  - onlyoffice
categories:
  - Greek
  - ONLYOFFICE
twitter_text: "Δημιουργία ONLYOFFICE χωρίς το όριο των 20 συνδέσεων"
---

![Δημιουργία ONLYOFFICE χωρίς το όριο των 20 συνδέσεων](/post_images/onlyoffice/ONLYOFFICE-20-connections.png "Δημιουργία ONLYOFFICE χωρίς το όριο των 20 συνδέσεων")

## Εισαγωγή

- Έχουν μείνει τα αποθετήρια του btactic-oo σε μια παλιά ημερομηνία στο παρελθόν;
- Χρειάζεστε να δημιουργήσετε μια νεότερη έκδοση onlyoffice από αυτή που κυκλοφόρησε το αποθετήριο `unlimited-onlyoffice-package-builder` του btactic-oo;
- Θέλετε να έχετε ένα απεριόριστο onlyoffice repo αντί του προεπιλεγμένου που περιορίζεται σε 20 συνδέσεις;
- Θέλετε να χρησιμοποιήσετε το Github Actions ώστε να γνωρίζετε ότι έχετε τους πόρους για να δημιουργήσετε τα πάντα;

Λοιπόν, σε αυτή την περίπτωση αυτό το άρθρο είναι για εσάς.

Θα είστε σε θέση να παράγετε ένα πακέτο Debian 11 DEB που μπορεί επίσης να λειτουργήσει και σε άλλα συστήματα Debian/Ubuntu.

Αν έχετε ήδη ακολουθήσει αυτές τις οδηγίες και επαναχρησιμοποιείτε τη μηχανή **DESKTOPM** και θέλετε να δημιουργήσετε μια νεότερη έκδοση ONLYOFFICE, μπορείτε να μεταβείτε στην ενότητα: [Ενημέρωση και Ανάκτηση των πιο πρόσφατων tags (DESKTOPM)](#update-and-fetch-newest-tags-desktopm).

## Σχετικά με τα αρχεία καταγραφής ανάπτυξης

Αυτή η συγκεκριμένη τεκμηρίωση δεν θα ενημερώνεται τόσο συχνά.
Θα βασίζεται στην τρέχουσα τελευταία απλοποιημένη τεκμηρίωση η οποία είναι του _2024-09_.
Παρακαλούμε ελέγξτε τον [φάκελο development_logs/](https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/blob/main/development_logs). Ίσως θελήσετε να επιλέξετε ορισμένα commits από εκεί αντί από αυτά του _2024-09_ που θα χρησιμοποιηθούν εδώ.

## Προαπαιτούμενα

Παρά αυτά που υποθέτουμε αμέσως παρακάτω, θα μπορούσατε στην πραγματικότητα να κάνετε τα πάντα στην ίδια μηχανή, είτε μια πραγματική μηχανή είτε μια εικονική μηχανή χάρη στο Docker.

Σε αυτό το άρθρο θα υποθέσουμε ότι έχετε:

- Ένα επιτραπέζιο μηχάνημα GNU/Linux όπου θα έχετε τα κλειδιά Github σας
- Μια κενή εικονική μηχανή με Debian 11 σε αυτήν, η οποία θα χρησιμοποιηθεί μόνο για την δημιουργία (build) του ONLYOFFICE
- Μια εικονική μηχανή με εγκατεστημένο ONLYOFFICE από τα πακέτα των επίσημων αποθετηρίων (repos)

ή με άλλα λόγια (με τις δικές τους ψευδώνυμες ονομασίες):

- Μια επιτραπέζια μηχανή GNU/Linux (**DESKTOPM**)
- Μια μηχανή GNU/Linux για δημιουργία (build) onlyoffice (**BUILDM**)
- Μια μηχανή με εγκατεστημένο ONLYOFFICE χάρη στα επίσημα πακέτα των αποθετηρίων (repos) (**ONLYM**)

## Εύρεση και αντικατάσταση

Βεβαιωθείτε ότι κατεβάσατε αυτό το έγγραφο, κάντε ένα αντίγραφό του και επεξεργαστείτε το.

Τώρα μπορείτε να βρείτε και να αντικαταστήσετε τα παρακάτω:

- Βρείτε όλες τις συμβολοσειρές `@@ACMEOO@@` και αντικαταστήστε τις με τον οργανισμό/χρήστη σας στο Github. Παράδειγμα: `acmeoo`.
- Βρείτε όλες τις συμβολοσειρές `@@ACME@@` και αντικαταστήστε τις με το brand σας (χωρίς κενά ή ιδιαίτερους χαρακτήρες). Παράδειγμα: `acme`. Αυτό θα χρησιμοποιηθεί τόσο για τα Git tags όσο και για το επίθημα του πακέτου Debian.
- Βρείτε όλες τις συμβολοσειρές `@@OOBUILDER@@` και αντικαταστήστε τις με τον χρήστη σας που έχει δικαιώματα docker. Παράδειγμα: `oobuilder`.
- Δοθείσας μιας έκδοσης **x.y.z.t** που θέλετε να δημιουργήσετε (build): ( Παράδειγμα: `8.1.2.3` )
  - Βρείτε όλες τις συμβολοσειρές `@@VERSION-X.Y.Z@@` και αντικαταστήστε τις με **x.y.z**. Παράδειγμα: `8.1.2`
  - Βρείτε όλες τις συμβολοσειρές `@@VERSION-T@@` και αντικαταστήστε τις με **t**. Παράδειγμα: `3`

---

## Github - Ωρα για Fork

### Github - Δημιουργία οργανισμού ή χρήστη

Πρώτα απ 'όλα πρέπει να δημιουργήσετε ένα λογαριασμό/χρήστη Github, έναν οργανισμό Github, ή να επαναχρησιμοποιήσετε τον υπάρχοντα λογαριασμό/χρήστη σας στο Github.

### Σύνδεση στον λογαριασμό σας στο Github

Θα πρέπει να γνωρίζετε πώς να συνδεθείτε στον λογαριασμό σας στο Github. Συνδεθείτε εκεί.

### Fork του unlimited-onlyoffice-package-builder από την btactic

- Επισκεφτείτε το [αποθετήριο unlimited-onlyoffice-package-builder της btactic-oo](https://github.com/btactic-oo/unlimited-onlyoffice-package-builder).
- Κάντε κλικ στο κουμπί **Fork**.
- Επιλέξτε `@@ACMEOO@@` ως τον Owner (ιδιοκτήτη).
- Αποεπιλέξτε την επιλογή 'Copy the main branch only' (Αντιγραφή μόνο του κύριου branch).
- **Μην τροποποιήσετε** το όνομα του αποθετηρίου (Repository name).
- Κάντε κλικ στο κουμπί **Create fork**.

### Fork του build_tools από την ONLYOFFICE

- Επισκεφτείτε το [αποθετήριο build_tools της ONLYOFFICE](https://github.com/ONLYOFFICE/build_tools).
- Κάντε κλικ στο κουμπί **Fork**.
- Επιλέξτε `@@ACMEOO@@` ως τον Owner (ιδιοκτήτη).
- Αποεπιλέξτε την επιλογή 'Copy the main branch only' (Αντιγραφή μόνο του κύριου branch).
- **Μην τροποποιήσετε** το όνομα του αποθετηρίου (Repository name).
- Κάντε κλικ στο κουμπί **Create fork**.

### Fork του server από την ONLYOFFICE

- Επισκεφτείτε το [αποθετήριο server της ONLYOFFICE](https://github.com/ONLYOFFICE/server).
- Κάντε κλικ στο κουμπί **Fork**.
- Επιλέξτε `@@ACMEOO@@` ως τον Owner (ιδιοκτήτη).
- Αποεπιλέξτε την επιλογή 'Copy the main branch only' (Αντιγραφή μόνο του κύριου branch).
- **Μην τροποποιήσετε** το όνομα του αποθετηρίου (Repository name).
- Κάντε κλικ στο κουμπί **Create fork**.

### Fork του web-apps από την ONLYOFFICE

- Επισκεφτείτε το [αποθετήριο web-apps της ONLYOFFICE](https://github.com/ONLYOFFICE/web-apps).
- Κάντε κλικ στο κουμπί **Fork**.
- Επιλέξτε `@@ACMEOO@@` ως τον Owner (ιδιοκτήτη).
- Αποεπιλέξτε την επιλογή 'Copy the main branch only' (Αντιγραφή μόνο του κύριου branch).
- **Μην τροποποιήσετε** το όνομα του αποθετηρίου (Repository name).
- Κάντε κλικ στο κουμπί **Create fork**.

## Προετοιμασία νέων τοπικών αποθετηρίων (DESKTOPM)

### Προειδοποίηση

Ίσως έχετε ένα προσαρμοσμένο φάκελο όπου δημιουργείτε (build) τα πάντα που σχετίζονται με το ONLYOFFICE. Σας συνιστώ να χρησιμοποιήσετε αυτό που σας δίνεται εδώ. Διαφορετικά θα πρέπει να βρείτε το `onlyoffice_repos` και να το αντικαταστήσετε με το δικό σας φάκελο, βεβαιωθείτε ότι χρησιμοποιείτε την ίδια δομή φακέλων.

### Github ssh keys

Βεβαιωθείτε ότι ο χρήστης σας έχει συσχετίσει τα [δικά του ssh κλειδιά με τον χρήστη σας στο Github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account). Θα τα χρειαστείτε για να κάνετε push σε αποθετήρια Github αργότερα.

### Κύριος φάκελος

{% highlight ruby %}
mkdir ~/onlyoffice_repos
{% endhighlight %}

### Κλώνος του δικού σας αποθετηρίου unlimited-onlyoffice-package-builder

Μπορείτε να παραλείψετε αυτό το βήμα, αλλά είναι ωραίο να έχετε τα πραγματικά αποθετήρια στον υπολογιστή σας για την περίπτωση που εξαφανιστούν.

{% highlight ruby %}
cd ~/onlyoffice_repos
git clone git@github.com:@@ACMEOO@@/unlimited-onlyoffice-package-builder.git
{% endhighlight %}

### Κλώνος του δικού σας αποθετηρίου build_tools

{% highlight ruby %}
cd ~/onlyoffice_repos
git clone git@github.com:@@ACMEOO@@/build_tools.git
{% endhighlight %}

### Κλώνος του δικού σας αποθετηρίου server

{% highlight ruby %}
cd ~/onlyoffice_repos
git clone git@github.com:@@ACMEOO@@/server.git
{% endhighlight %}

### Κλώνος του δικού σας αποθετηρίου web-apps

{% highlight ruby %}
cd ~/onlyoffice_repos
git clone git@github.com:@@ACMEOO@@/web-apps.git
{% endhighlight %}

## Προσθήκη upstream και btactic αποθετηρίων ως remotes (DESKTOPM)

Θα χρειαστεί να μπορούμε να κάνουμε fetch και από τα δύο upstream (ONLYOFFICE) και btactic αποθετήρια.
Από το upstream θα πάρουμε τα τελευταία tags (χρήσιμο αν επαναλάβετε αυτή τη διαδικασία στο μέλλον).
Και από τα btactic αποθετήρια θα πάρετε τα commits "χωρίς όρια", για την περίπτωση που δεν θέλετε να τα δημιουργήσετε ξανά χειροκίνητα.

Θα κάνουμε αυτό το βήμα ταυτόχρονα για όλα τα απαραίτητα αποθετήρια ώστε να μην καταλάβει πολύ χώρο στο έγγραφο.

{% highlight ruby %}
cd ~/onlyoffice_repos/build_tools
git remote add upstream-origin git@github.com:ONLYOFFICE/build_tools.git
git remote add btactic-origin git@github.com:btactic-oo/build_tools.git

cd ~/onlyoffice_repos/server
git remote add upstream-origin git@github.com:ONLYOFFICE/server.git
git remote add btactic-origin git@github.com:btactic-oo/server.git

cd ~/onlyoffice_repos/web-apps
git remote add upstream-origin git@github.com:ONLYOFFICE/web-apps.git
git remote add btactic-origin git@github.com:btactic-oo/web-apps.git
{% endhighlight %}

## Ενημέρωση και Ανάκτηση των πιο πρόσφατων tags (DESKTOPM)

<a id="update-and-fetch-newest-tags-desktopm"></a>

Για άλλη μια φορά το κάνουμε μονομιάς.

{% highlight ruby %}
cd ~/onlyoffice_repos/build_tools
git checkout master
git pull upstream-origin master
git fetch --all --tags

cd ~/onlyoffice_repos/server
git checkout master
git pull upstream-origin master
git fetch --all --tags

cd ~/onlyoffice_repos/web-apps
git checkout master
git pull upstream-origin master
git fetch --all --tags
{% endhighlight %}

## Προαπαιτούμενα (ONLYM)

Αυτή η μηχανή **ONLYM** πρέπει να έχει εγκατεστημένο το [περιορισμένο deb πακέτο onlyoffice από τα επίσημα αποθετήρια](https://helpcenter.onlyoffice.com/installation/docs-community-install-ubuntu.aspx).

## Εντοπισμός του tag για δημιουργία (build) (ONLYM)

Η εικονική μηχανή ONLYOFFICE μας θα πρέπει να έχει ήδη εγκατεστημένο το πακέτο Debian σε αυτήν.
Οι προγραμματιστές του ONLYOFFICE δεν ενημερώνουν ποτέ την προεπιλεγμένη έκδοση, οπότε μην μπείτε στον κόπο να την ενημερώσετε ή οποιονδήποτε από τους φακέλους που χρησιμοποιεί.

{% highlight ruby %}
sudo apt update
sudo apt-cache show onlyoffice-documentserver | less
{% endhighlight %}

Η πιο πρόσφατη έκδοση @@VERSION-X.Y.Z@@ είναι:

@@VERSION-X.Y.Z@@-@@VERSION-T@@

οπότε αυτή είναι η έκδοση που θα χρησιμοποιήσουμε.
Απλώς αντικαθιστούμε την παύλα με μια τελεία. Το @@VERSION-X.Y.Z@@-@@VERSION-T@@ είναι τώρα: @@VERSION-X.Y.Z@@.@@VERSION-T@@.

## Εφαρμογή των no-limits στα αποθετήρια μας (DESKTOPM)

### Ενημέρωση αποθετηρίου build_tools

Παλιά πράγματα που έχουμε ήδη από τα btactic repos:

- commit (αλλαγές ιδιοκτήτη σε ssh): 7ce465ecb177fd20ebf2b459a69f98312f7a8d3d
- commit (Custom repos and tags): 7da607da885285fe3cfc9feaf37b1608666039eb

Δημιουργούμε ένα νέο branch βασισμένο στο πρόσφατα ληφθέν tag.

{% highlight ruby %}
cd ~/onlyoffice_repos/build_tools

git checkout tags/v@@VERSION-X.Y.Z@@.@@VERSION-T@@ -b @@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@
{% endhighlight %}

Κάντε cherry-pick ό,τι είχαμε ήδη:

{% highlight ruby %}
git cherry-pick 7ce465ecb177fd20ebf2b459a69f98312f7a8d3d
git cherry-pick 7da607da885285fe3cfc9feaf37b1608666039eb
{% endhighlight %}

Βρείτε και αντικαταστήστε τον οργανισμό btactic και το επίθημά του με το δικό μας:

{% highlight ruby %}
sed -i 's/unlimited_organization = "btactic-oo"/unlimited_organization = "@@ACMEOO@@"/g' scripts/base.py

sed -i 's/unlimited_tag_suffix = "-btactic"/unlimited_tag_suffix = "-@@ACME@@"/g' scripts/base.py
{% endhighlight %}

Τροποποιήστε το τελευταίο commit για να χρησιμοποιήσει τα δικά μας tags.

{% highlight ruby %}
git add scripts/base.py
git commit --amend --no-edit
{% endhighlight %}

Ας κάνουμε push και ας δημιουργήσουμε τα κατάλληλα tags:

{% highlight ruby %}
git push origin @@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@

git tag -a 'v@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@' -m '@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@'

git push origin v@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@
{% endhighlight %}

### Ενημέρωση αποθετηρίου server

Παλιά πράγματα που υπάρχουν ήδη από τα btactic repos:

- commit (connection limit): cb6100664657bc91a8bae82d005f00dcc0092a9c

Δημιουργούμε ένα νέο branch βασισμένο στο πρόσφατα ληφθέν tag.

{% highlight ruby %}
cd ~/onlyoffice_repos/server

git checkout tags/v@@VERSION-X.Y.Z@@.@@VERSION-T@@ -b @@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@
{% endhighlight %}

Κάντε cherry-pick ό,τι είχαμε ήδη:

{% highlight ruby %}
git cherry-pick cb6100664657bc91a8bae82d005f00dcc0092a9c
{% endhighlight %}

Ας κάνουμε push και ας δημιουργήσουμε τα κατάλληλα tags:

{% highlight ruby %}
git push origin @@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@

git tag -a 'v@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@' -m '@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@'

git push origin v@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@
{% endhighlight %}

### Ενημέρωση αποθετηρίου web-apps

Παλιά πράγματα που υπάρχουν ήδη από τα btactic repos:

- commit (mobile edit): 2d186b887bd1f445ec038bd9586ba7da3471ba05

Δημιουργούμε ένα νέο branch βασισμένο στο πρόσφατα ληφθέν tag.

{% highlight ruby %}
cd ~/onlyoffice_repos/web-apps

git checkout tags/v@@VERSION-X.Y.Z@@.@@VERSION-T@@ -b @@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@
{% endhighlight %}

Κάντε cherry-pick ό,τι είχαμε ήδη:

{% highlight ruby %}
git cherry-pick 2d186b887bd1f445ec038bd9586ba7da3471ba05
{% endhighlight %}

Ας κάνουμε push και ας δημιουργήσουμε τα κατάλληλα tags:

{% highlight ruby %}
git push origin @@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@

git tag -a 'v@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@' -m '@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@'

git push origin v@@VERSION-X.Y.Z@@.@@VERSION-T@@-@@ACME@@
{% endhighlight %}

## Αποφασίστε πού θα κάνετε το build

Αν θέλετε να κάνετε build στο δικό σας VPS, παρακαλούμε ελέγξτε:

- [Ρύθμιση μηχανής build (BUILDM)](#build-machine-setup-buildm)

Αν θέλετε να κάνετε build στο Github, παρακαλούμε ελέγξτε:

- [Κυκλοφορία (Release) βάσει Github Actions (DESKTOPM)](#release-based-on-github-actions-desktopm)

## Ρύθμιση μηχανής build (BUILDM)

<a id="build-machine-setup-buildm"></a>

### Σχετικά με την ενότητα ρύθμισης μηχανής build

Παρακαλούμε σημειώστε ότι αν αποφασίσετε να κάνετε build απευθείας από το Github Actions, αυτή η μηχανή build δεν θα χρειαστεί καθόλου, οπότε μπορείτε να παραλείψετε εντελώς αυτή την ενότητα.

### Προαπαιτούμενα

- Έχει επιλεγεί Debian 11 Netinst (Οποιαδήποτε άλλη διανομή βασισμένη σε Debian που υποστηρίζει docker θα πρέπει επίσης να είναι εντάξει).
- Απαιτούμενη RAM: 16 GB RAM (Ελάχιστο) ή 8 GB RAM με 8 GB SWAP.
- Συνιστώμενο: 50 GB χώρος στο δίσκο.

### Προαπαιτούμενο Docker-CE

Αυτή η μέθοδος build χρησιμοποιεί το Docker κάτω από το κέλυφος. Θα βρείτε οδηγίες για το πώς να ρυθμίσετε τον χρήστη σας για build ώστε να χρησιμοποιεί το Docker. Αυτό χρειάζεται να γίνει μόνο μία φορά. Αυτές οι οδηγίες Docker προορίζονται για Ubuntu 20.04, αλλά οποιεσδήποτε άλλες γενικές οδηγίες ρύθμισης Docker για το λειτουργικό σας σύστημα θα πρέπει να είναι εντάξει.

Να είστε ενήμεροι για διανομές που βασίζονται σε RHEL 8. Αναζητήστε ένα [howto για docker-ce](https://computingforgeeks.com/install-docker-and-docker-compose-on-rhel-8-centos-8/). Η προσπάθεια εγκατάστασης του πακέτου docker απευθείας εγκαθιστά το _podman_ και το _buildah_ τα οποία **δεν λειτουργούν ακριβώς όπως το docker-ce** αν και φαίνεται να διαφημίζονται ως τέτοια.

### Ρύθμιση Docker

_Σημείωση: Οι εντολές για αυτή τη ρύθμιση Docker πρέπει να εκτελούνται είτε ως χρήστης root είτε ως χρήστης που είναι μέρος της ομάδας sudo, συνήθως ο χρήστης admin._

#### Εγκατάσταση προαπαιτούμενων docker

{% highlight ruby %}
sudo apt-get update
sudo apt-get remove docker docker-engine docker.io
sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
{% endhighlight %}

#### Ρύθμιση του αποθετηρίου apt του docker

{% highlight ruby %}
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo tee /etc/apt/sources.list.d/docker.list <<EOM
deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable
EOM

sudo apt-get update
{% endhighlight %}

#### Εγκατάσταση docker

{% highlight ruby %}
sudo apt-get install docker-ce
{% endhighlight %}

### Χρήστης Docker - Δημιουργία

{% highlight ruby %}
sudo usermod -a -G docker @@OOBUILDER@@
{% endhighlight %}

### Χρήστης Docker - Επανείσοδος

Για να μπορείτε να χρησιμοποιήσετε σωστά το Docker από τον χρήστη `@@OOBUILDER@@`, ίσως χρειαστεί να κάνετε logout και μετά login στον χρήστη σας.
Μπορεί να βρείτε πώς να επιβάλλετε τα δικαιώματα της ομάδας Docker του χρήστη χωρίς να κάνετε logout αν ψάξετε αρκετά, αλλά τις περισσότερες φορές είναι ευκολότερο να κάνετε απλώς logout και login.

### Χρήστης Docker - Hello world

Επίσης, βεβαιωθείτε ότι εκτελείτε τα συνηθισμένα παραδείγματα docker 'Hello world' κάτω από τον χρήστη `@@OOBUILDER@@`.
Αυτά τα παραδείγματα docker 'Hello world' εξηγούνται συνήθως στις περισσότερες εγχειρίδια εγκατάστασης docker.
Εάν το παράδειγμα docker 'Hello world' δεν λειτουργεί όπως αναμένεται, τότε η δημιουργία (building) χάρη στα Dockerfiles μας σίγουρα δεν θα λειτουργήσει.

### Git ssh keys

_Σημείωση: Οι παρακάτω εντολές πρέπει να εκτελεστούν ως ο χρήστης `@@OOBUILDER@@`._

Πρέπει να εκτελέσετε την παρακάτω εντολή για να δημιουργήσετε ένα κλειδί.

{% highlight ruby %}
ssh-keygen -t rsa -b 4096 -C "@@OOBUILDER@@@domain.com"
{% endhighlight %}

Η διεύθυνση email πρέπει να είναι αυτή που χρησιμοποιείτε για τον λογαριασμό σας στο GitHub.

Στη συνέχεια, ανεβάστε το κλειδί `id_rsa.pub` στο προφίλ σας στο GitHub: [https://github.com/settings/keys](https://github.com/settings/keys).

Σημείωση: Προσωπικά χρησιμοποιώ μόνο έναν επιπλέον λογαριασμό Github, γιατί δεν μπορείτε να ορίσετε αυτό το SSH κλειδί ως read-only. Υποτίθεται ότι πρέπει να χρησιμοποιήσετε ένα deploy key, αλλά αυτά συνδέονται με ένα μόνο αποθετήριο ή οργανισμό.

### Λογισμικό Git

{% highlight ruby %}
apt install git
{% endhighlight %}

θα πρέπει να αρκεί στα περισσότερα συστήματα Debian/Ubuntu ώστε να μπορείτε αργότερα να χρησιμοποιήσετε το Git.

## Build (BUILDM)

### Build τα πάντα

Ως ο χρήστης `@@OOBUILDER@@` εκτελέστε:

{% highlight ruby %}
mkdir ~/build-oo

cd ~/build-oo

git clone https://github.com/@@ACMEOO@@/unlimited-onlyoffice-package-builder

cd unlimited-onlyoffice-package-builder

git checkout v0.0.1

# Αγνοήστε το μήνυμα detached HEAD

./onlyoffice-package-builder.sh --product-version=@@VERSION-X.Y.Z@@ --build-number=@@VERSION-T@@ --unlimited-organization=@@ACMEOO@@ --tag-suffix=-@@ACME@@ --debian-package-suffix=-@@ACME@@
{% endhighlight %}

### Τελικό πακέτο deb

Το τελικό πακέτο deb `onlyoffice-documentserver_@@VERSION-X.Y.Z@@-@@VERSION-T@@-@@ACME@@_amd64.deb` μπορεί να βρεθεί στον φάκελο: `~/build-oo/unlimited-onlyoffice-package-builder/document-server-package/deb/`.

Αν θέλατε να κάνετε build στο δικό σας VPS, **τελειώσατε.**

## Κυκλοφορία (Release) βάσει Github Actions (DESKTOPM)

<a id="release-based-on-github-actions-desktopm"></a>

### Ενεργοποίηση Github Actions

Επισκεφτείτε το [https://github.com/@@ACMEOO@@/unlimited-onlyoffice-package-builder/actions](https://github.com/@@ACMEOO@@/unlimited-onlyoffice-package-builder/actions) και κάντε κλικ στο κουμπί **I understand my workflows, go ahead and enable them**.

### Χρήση των αποθετηρίων σας κατά την εκτέλεση Github Actions

{% highlight ruby %}
cd ~/onlyoffice_repos/unlimited-onlyoffice-package-builder
git checkout main

sed -i 's/DEBIAN_PACKAGE_SUFFIX: -btactic/DEBIAN_PACKAGE_SUFFIX: -@@ACME@@/g' .github/workflows/build-release-debian-11.yml

sed -i 's/TAG_SUFFIX: -btactic/TAG_SUFFIX: -@@ACME@@/g' .github/workflows/build-release-debian-11.yml

git add .github/workflows/build-release-debian-11.yml

git commit -m 'Use @@ACME@@ as a suffix in Github Actions'

git push origin main
{% endhighlight %}

### Push για build

{% highlight ruby %}
cd ~/onlyoffice_repos/unlimited-onlyoffice-package-builder

git checkout main

# Μόνο για ασφάλεια

git push origin main

git tag -a 'builds-debian-11/@@VERSION-X.Y.Z@@.@@VERSION-T@@' -m 'builds-debian-11/@@VERSION-X.Y.Z@@.@@VERSION-T@@'

git push origin 'builds-debian-11/@@VERSION-X.Y.Z@@.@@VERSION-T@@'
{% endhighlight %}

Η κυκλοφορία (Release) βάσει Github Actions, την οποία μπορείτε να ελέγξετε στη διεύθυνση: [https://github.com/@@ACMEOO@@/unlimited-onlyoffice-package-builder/actions](https://github.com/@@ACMEOO@@/unlimited-onlyoffice-package-builder/actions) θα πρέπει να ολοκληρωθεί επιτυχώς μετά από περίπου 2 ώρες και 30 λεπτά χρόνου build.

Ελέγξτε τη νέα κυκλοφορία στη διεύθυνση: [https://github.com/@@ACMEOO@@/unlimited-onlyoffice-package-builder/releases](https://github.com/@@ACMEOO@@/unlimited-onlyoffice-package-builder/releases).

Αν θέλατε να κάνετε build στο Github, **τελειώσατε.**

---

## Συμβουλές

- Αν θέλετε ανατροφοδότηση, βεβαιωθείτε ότι περιγράφετε:
  - Πώς εγκαθίσταται το docker στο VPS σας
  - Ποιες είναι οι ακριβείς εντολές που εκτελέσατε
- Οι προγραμματιστές του ONLYOFFICE δεν χρησιμοποιούν τα δημόσια εργαλεία που δημοσιεύονται εδώ για να δημιουργήσουν τα binaries τους, χρησιμοποιούν ένα άλλο σύνολο εργαλείων τα οποία, θεωρητικά, θα πρέπει να λειτουργούν αρκετά παρόμοια.
- Σας συμβουλεύεται κάπου στην τεκμηρίωση build να χρησιμοποιήσετε το master branch, αλλά θα πρέπει να μείνετε σε ένα tag, ώστε να παίρνετε πάντα τα ίδια αποτελέσματα.
- Δυστυχώς πολλές εξωτερικές εξαρτήσεις του ONLYOFFICE βασίζονται όχι σε tags (συγκεκριμένες εκδόσεις) αλλά σε master branches. **Αυτό είναι ένα μεγάλο σφάλμα από την πλευρά τους.**
- Έτσι, ακόμα και αν καταφέρετε να δημιουργήσετε ONLYOFFICE μια μέρα χωρίς να αλλάξουν τα ONLYOFFICE αποθετήρια, μπορεί να αποτύχει την επόμενη μέρα λόγω κάποιας εξωτερικής εξάρτησης που άλλαξε πολύ ή είναι προσωρινά εκτός λειτουργίας.
- Αν ζείτε σε χώρες παρόμοιες με τη Ρωσία ή την Κίνα, ενδέχεται να μην έχετε πρόσβαση σε κάποιες από αυτές τις εξωτερικές εξαρτήσεις λόγω θεμάτων γεωγραφικού αποκλεισμού.
- Το ίδιο μπορεί να συμβεί αν δεν έχετε σταθερή σύνδεση internet. Το σύστημα build δεν επιτρέπει την εύκολη επανεκτέλεση μόνο του τελευταίου βήματος που απέτυχε λόγω χρονικού ορίου διαδικτύου (Internet timeout).
- Πρέπει επίσης να γνωρίζετε ότι όταν ένα βήμα build έχει αποτύχει, είναι πολύ καλύτερο για εσάς να ξεκινήσετε από την αρχή παρά να προσπαθήσετε να συνεχίσετε το build. Αυτή η αποτυχία μπορεί να σας εμποδίσει να δημιουργήσετε τα πάντα εντάξει ξανά. Λοιπόν, τουλάχιστον δοκιμάστε το άλλη μία φορά από έναν κενό φάκελο.
- Χρειάζεστε αρκετή RAM. Είναι γνωστό ότι αποτυγχάνει με μόνο 4 GB RAM. Σε εμένα λειτουργεί με 16 GB RAM.

## Προειδοποίηση

Αυτό δεν είναι ένα επίσημο build του ONLYOFFICE. Μην αναζητήσετε βοήθεια στα ζητήματα/φόρουμ της ONLYOFFICE, εκτός αν αναπαράγετε το πρόβλημα στον αρχικό πηγαίο κώδικα ή στα αρχικά binaries από αυτούς.

## Χρήσιμοι σύνδεσμοι

- [https://www.btactic.com/build-onlyoffice-from-source-code-2023/?lang=en](https://www.btactic.com/build-onlyoffice-from-source-code-2023/?lang=en)
- [https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/tree/main/development_logs](https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/tree/main/development_logs)

## Ανατροφοδότηση

Παρακαλούμε [δημιουργήστε ένα νέο ζήτημα για την εμπειρία σας](https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/issues) (με τις εφαρμοσμένες λύσεις σας για μια συγκεκριμένη έκδοση onlyoffice) ώστε να μάθουμε όλοι από την εμπειρία σας.

### ΠΗΓΕΣ

Github: [https://github.com/btactic-oo/unlimited-onlyoffice-package-builder](https://github.com/btactic-oo/unlimited-onlyoffice-package-builder)

Github, how to build [https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/blob/main/README-BUILD-NEWER-VERSIONS.md](https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/blob/main/README-BUILD-NEWER-VERSIONS.md) (μεταφρασμένο)

Build OnlyOffice from source code (2024-05) [https://www.btactic.com/build-onlyoffice-from-source-code-2024-05/](https://www.btactic.com/build-onlyoffice-from-source-code-2024-05/)

Github development logs [https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/tree/main/development_logs](https://github.com/btactic-oo/unlimited-onlyoffice-package-builder/tree/main/development_logs)

Forum του Nextcloud [https://help.nextcloud.com/t/onlyoffice-server-with-no-limits-2024/192498](https://help.nextcloud.com/t/onlyoffice-server-with-no-limits-2024/192498)

OnlyOffice docker image with mobile editing and without concurrent user limit [https://github.com/yvvidolov/Docker-DocumentServer](https://github.com/yvvidolov/Docker-DocumentServer)

---

## Αρχική δημοσίευση:

[https://eiosifidis.blogspot.com/2025/04/only-office-no-limits.html](https://eiosifidis.blogspot.com/2025/04/only-office-no-limits.html){:target="\_blank"}
