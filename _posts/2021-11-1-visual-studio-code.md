---
layout: post
title: "ΠΛΗΡΗΣ ΟΔΗΓΟΣ εγκατάστασης και ρυθμίσεων του Visual Studio Code σε Linux, για τις γλώσσες python, C και java"
date: 2021-11-1 12:00:00
description: ΣΧΕΔΟΝ ΠΛΗΡΗΣ ΟΔΗΓΟΣ, εγκατάστασης και ρύθμισης του Visual Studio Code σε Linux για python, C και java.
tags:
- visual studio code
- vscode
- openSUSE
- ubuntu
- linux mint
- arch linux
- java
- python
- C
- jupyter
categories:
- Visual Studio Code
- vscode
- Arch Linux
- Ubuntu
- Linux Mint
- openSUSE
- Java
- Python
- C
- Jupyter
twitter_text: 'ΠΛΗΡΗΣ ΟΔΗΓΟΣ εγκατάστασης και ρυθμίσεων του Visual Studio Code σε Linux, για τις γλώσσες python, C και java'
---

![Visual Studio Code](/post_images/vscode/Visual_Studio_Code.png "Visual Studio Code"){:height="200px" width="200px"}


# ΠΡΟΛΟΓΟΣ

Έχουμε δει σε προηγούμενο άρθρο την [Εγκατάσταση του Visual Studio Code σε openSUSE](https://eiosifidis.blogspot.com/2019/12/visual-code-opensuse.html). Σε αυτό το άρθρο θα δείξουμε λίγο πιο ολοκληρωμένες ενέργειες. Αφορμή της συγγραφής του άρθρου αυτού ήταν η αποτυχία ρύθμισης και χρήσης του Visual Studio Code με τις γλώσσες προγραμματισμού Python, C και Java. Η αρχική εγκατάσταση έγινε μέσω flatpak (θα το δουμε παρακάτω) αλλά δεν μπορούσε να γίνει ρύθμιση με Java. Είχα ψάξει στα γρήγορα τι είναι καλύτερο για το VSCode, το flatpak ή το αρχείο deb/rpm και [βρήκα αυτό στο reddit](https://www.reddit.com/r/elementaryos/comments/gtcibj/flatpakflathub_vs_app_store_or_deb_file_vscode/). Σίγουρα υπάρχουν και άλλα αποτελέσματα. Εδώ κάποιος ανέφερε ότι στο flatpak υπάρχουν κάποια θέματα debugging οπότε αποφάσισα και εγώ να κάνω εγκατάσταση από deb/rpm. Όμως έπρεπε να κρατήσω και τα extensions που έχω εγκατεστημένα (για να μην ψάχνω). Βρήκα και τρόπο γι'αυτό. Οπότε εδώ θα προσπαθήσω να τα αναφέρω όλα.  

## ΕΓΚΑΤΑΣΤΑΣΗ Visual Studio Code

Θα δούμε τους πιο γνωστούς τρόπους εγκατάστασης του Visual Studio Code.  

### ΕΓΚΑΤΑΣΤΑΣΗ με αρχεία deb/rpm

Για διανομές όπως είναι η openSUSE, Fedora, Red Hat, Ubuntu, Debian και παράγωγά τους, μπορείτε να κατεβάσετε το αντίστοιχο αρχείο για τη διανομή σας (rpm για τις 3 πρώτες και deb για τις άλλες 2). Κάντε την λήψη των αρχείων από εδώ:  

[![Download](/post_images/download.png "Download")](https://code.visualstudio.com/Download)

Η εγκατάσταση είναι απλή. Μπορείτε με διπλό κλικ πάνω στο αρχείο, οπότε θα ανοίξει ο διαχειριστής αρχείων εγκατάστασης της διανομής σας και στη συνεχεια θα πατήσετε το κουμπί εγκατάστασης. Εναλλακτικά μπορείτε να το εγκαταστήσετε με τερματικό με τις εντολές:  

{% highlight ruby %}
**Για Ubuntu, Debian:**  
sudo dpkg -i code*.deb  

**Για Fedora, Red Hat, openSUSE:**  
sudo rpm -i code*.rpm
{% endhighlight %} 

Σε [Arch Linux](https://wiki.archlinux.org/title/Visual_Studio_Code) θα βρείτε πολλές εκδόσεις. Προτιμήστε την έκδοση **visual-studio-code-bin** που θα την βρείτε από το αποθετήριο AUR (ανάλογα με ποιον AUR helper χρησιμοποιείτε, αλλάξτε την εντολή).  

{% highlight ruby %}
yay -S visual-studio-code-bin
{% endhighlight %} 

Υπάρχουν περισσότερες πληροφορίες πως μπορείτε να προσθέσετε αποθετήριο και να το κάνετε εγκατάσταση χειροκίνητα για όλες τις διανομές στην [επίσημη τεκμηρίωση εδώ](https://code.visualstudio.com/docs/setup/linux).  

### ΕΓΚΑΤΑΣΤΑΣΗ με flatpak

Εάν έχετε εγκατεστημένο το flatpak στον υπολογιστή σας ([δείτε πως γίνεται αυτό](https://eiosifidis.blogspot.com/2019/12/opensuse-flatpak.html)), μπορείτε να εγκαταστήσετε το Visual Studio Code με την παρακάτω εντολή:  

{% highlight ruby %}
flatpak install com.visualstudio.code
{% endhighlight %} 

Αν και έχει άλλες δυο εκδόσεις του Visual Studio Code (vscodium και code-OSS), επιλέξτε την παραπάνω έκδοση.  

### ΕΓΚΑΤΑΣΤΑΣΗ με snap

Οι διανομές που χρησιμοποιούν τα πακέτα snap (Ubuntu κυρίως), μπορείτε να το εγκαταστήσετε είτε από την [σελίδα snapcraft](https://snapcraft.io/code) είτε με την παρακάτω εντολή:  

{% highlight ruby %}
sudo snap install --classic code
{% endhighlight %} 

Εάν δεν έχετε snap εγκατεστημένο, δείτε στην διανομή σας πως εγκαθίσταται. Παράδειγμα, δείτε στην [openSUSE εδώ](https://iosifidis.github.io/energopoiisi-flatpak-snap-opensuse/).  

## ΕΓΚΑΤΑΣΤΑΣΗ προσθέτων για το λειτουργικό

### ΕΓΚΑΤΑΣΤΑΣΗ για Python

Για την εγκατάσταση της Python δεν θα χρειαστεί να κάνουμε κάτι γιατί είναι ήδη εγκατεστημένη στο σύστημα. Συνήθως είναι και κάποια έκδοση 2.x και κάποια έκδοση 3.x. Δείτε τις εκδόσεις με τις εντολές:  

{% highlight ruby %}
**Για python 2:**  
python --version  

**Για python 3:**  
python3 --version
{% endhighlight %} 

Ενώ μπορείτε να κάνετε αναβάθμιση του pip με την εντολή:  

{% highlight ruby %}
pip install --upgrade pip
{% endhighlight %} 

Ενώ για αναβάθμιση όλων των πακέτων που έχετε εγκατεστημένα, μπορείτε να εισάγετε την εντολή:  

{% highlight ruby %}
pip3 list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip3 install -U
{% endhighlight %} 

### ΕΓΚΑΤΑΣΤΑΣΗ για C

Και εδώ δεν θα χρειαστούν πολλά γιατί είναι ήδη εγκατεστημένη στον υπολογιστή σας. Πρέπει να είναι εγκατεστημένο το πακέτο **gcc**. Αυτό βρίσκεται στο build-essential

{% highlight ruby %}
**Για Ubuntu/Debian:**  
sudo apt install build-essential  

**Για openSUSE:**  
sudo zypper in gcc  

**Για Arch Linux:**  
sudo packam -S gcc
{% endhighlight %} 

Για να δείτε ποιά έκδοση μεταγλωττιστή έχετε, μπορείτε να χρησιμοποιήσετε την εντολή:  

{% highlight ruby %}
gcc --version
{% endhighlight %} 

Δοκιμάστε το κλασικό Hello world. Γράψτε στο τερματικό **nano hello.c** και [εισάγετε τον κώδικα για την C](https://www.programiz.com/c-programming/examples/print-sentence). Στη συνέχεια εκτελέστε τις εντολές:  

{% highlight ruby %}
$ gcc -o hello hello.c $ ./hello
{% endhighlight %} 

### ΕΓΚΑΤΑΣΤΑΣΗ για Java

Πριν ξεκινήσουμε την εγκατάσταση, ας δούμε τις διαφορές μεταξύ JRE, OpenJDK και Oracle JDK.  

*   **JRE (Java Runtime Environment)** είναι αυτό που χρειάζεται για να εκτελεστεί μια εφαρμογή που βασίζεται σε Java. Αυτό είναι το μόνο που χρειάζεστε εάν δεν είστε προγραμματιστής.
*   **JDK (Java Development Kit)** είναι αυτό που πρέπει να αναπτύξετε λογισμικό που σχετίζεται με την Java.
*   H **[OpenJDK](https://openjdk.java.net/)** είναι υλοποίηση ανοικτού κώδικα του **Java Development Kit** ενώ το **[Oracle JDK](https://www.oracle.com/java/technologies/javase-downloads.html)** είναι η επίσημη έκδοση Oracle του Java Development Kit. Ενώ το OpenJDK είναι αρκετό για τις περισσότερες περιπτώσεις, ορισμένα προγράμματα όπως το [Android Studio](https://developer.android.com/studio) προτείνει τη χρήση του [Oracle JDK](https://www.oracle.com/java/technologies/javase-downloads.html) για αποφυγή ζητήματος διεπαφής χρήστη.

Εδώ θα χρειαστεί να γίνουν κάποιες εγκαταστάσεις-ρυθμίσεις.  

Για αρχή δείτε τι έκδοση java έχετε.  

{% highlight ruby %}
java -version
{% endhighlight %} 

Εάν έχετε εγκατεστημένη την java, τότε σε ένα σύστημα Ubuntu θα δείτε τα παρακάτω:

{% highlight ruby %}
openjdk version "11.0.11" 2021-04-20  
OpenJDK Runtime Environment (build 11.0.11+9-Ubuntu-0ubuntu2.20.04)  
OpenJDK 64-Bit Server VM (build 11.0.11+9-Ubuntu-0ubuntu2.20.04, mixed mode, sharing)
{% endhighlight %} 

Ενώ σε ένα σύστημα που δεν έχει εγκατεστημένη την java, θα δείτε ένα αποτέλεσμα του τύπου:  

{% highlight ruby %}
The program ‘java’ can be found in the following packages:  
* default-jre  
* gcj-4.6-jre-headless  
* openjdk-6-jre-headless  
* gcj-4.5-jre-headless  
* openjdk-7-jre-headless  
Try: sudo apt-get install
{% endhighlight %} 

#### Εγκατάσταση JRE

Για εγκατάσταση του Java Runtime Environment  

{% highlight ruby %}
sudo apt install default-jre
{% endhighlight %} 

#### Εγκατάσταση OpenJDK

Για εγκατάσταση του OpenJDK  

{% highlight ruby %}
sudo apt install default-jdk
{% endhighlight %} 

#### Εγκατάσταση openSUSE

Στο openSUSE να έχετε εγκατεστημένα τα παρακάτω:  

{% highlight ruby %}
sudo zypper in java-11-openjdk-devel java-11-openjdk java-11-openjdk-headless
{% endhighlight %} 

#### Εγκατάσταση Oracle JDK

Για εγκατάσταση του [Oracle JDK](https://www.oracle.com/java/technologies/javase-downloads.html), κατεβάστε και εγκαταστείστε το αντίστοιχο αρχείο για την διανομή σας.  

Εναλλακτικά σε Ubuntu based διανομές, μπορείτε να εισάγετε το αποθετήριο:  

{% highlight ruby %}
sudo add-apt-repository ppa:webupd8team/java  
sudo apt-get update
{% endhighlight %} 

Και στη συνέχεια για την java έκδοση 16 (αυτή κυκλοφορεί τελευταία. Αν θέλετε να αλλάξετε έκδοση, απλά αλλάξτε το νούμερο), μπορείτε να την εγκαταστήσετε με τις παρακάτω εντολές:  

{% highlight ruby %}
sudo apt install oracle-java16-installer  
sudo apt install oracle-java16-set-default
{% endhighlight %} 

Για [openSUSE, υπάρχει διαθέσιμη τεκμηρίωση](https://en.opensuse.org/SDB:Installing_Java).  

Για Arch Linux, υπάρχει και [τεκμηρίωση στο wiki](https://wiki.archlinux.org/title/java).  

## ΡΥΘΜΙΣΕΙΣ Visual Studio Code

### Εγκαταστάσεις extensions

Για την πλήρη εμεπειρία χρήσης αλλά και για την εκτέλεση των προγραμμάτων, θα χρειαστεί να εγκατασταθούν κάποια πρόσθετα. Ποια ειναι αυτά και για ποιες γλώσσες προγραμματισμού;  

### ΕΓΚΑΤΑΣΤΑΣΗ για Python

Για την Python χρειάζεται το πρόσθετο που μπορείτε να βρείτε στο [Market place](https://marketplace.visualstudio.com/items?itemName=ms-python.python).  

Ανοίξτε το VS Code Quick Open (με τα πλήκτρα Ctrl+P) και επικολήστε την εντολή:  

{% highlight ruby %}ext install ms-python.python{% endhighlight %} 

Εναλλακτικά ανοίξτε την μηχανή αναζήτησης των extensions και αναζητήστε Python. Θα σας εμφανίσει πολλά. Εσείς επιλέξτε εκδότη την Microsoft (μπορείτε να επιλέξετε ότι άλλο θέλετε αλλά αυτό εφαρμόζει καλύτερα στο πρόγραμμα Visual Studio Code).  

Καλό είναι να εγκαταστήσετε και το [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter), το [Visual Studio IntelliCode](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode) και το [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance).  

Ανοίξτε το VS Code Quick Open (με τα πλήκτρα Ctrl+P) και επικολήστε την εντολή:  

{% highlight ruby %}
**Jupyter:**  
ext install ms-toolsai.jupyter  

**Visual Studio IntelliCode:**  
ext install VisualStudioExptTeam.vscodeintellicode  

**Pylance:**  
ext install ms-python.vscode-pylance
{% endhighlight %} 

Δείτε περισσότερα στην [τεκμηρίωση στην σελίδα του Visual Studio Code](https://code.visualstudio.com/docs/languages/python).  

### ΕΓΚΑΤΑΣΤΑΣΗ για C

Για την C χρειάζεται το πρόσθετο που μπορείτε να βρείτε στο [Market place](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).  

Ανοίξτε το VS Code Quick Open (με τα πλήκτρα Ctrl+P) και επικολήστε την εντολή:  

{% highlight ruby %}
ext install ms-vscode.cpptools
{% endhighlight %} 

Εναλλακτικά ανοίξτε την μηχανή αναζήτησης των extensions και αναζητήστε C/C++. Θα σας εμφανίσει πολλά. Εσείς επιλέξτε εκδότη την Microsoft (μπορείτε να επιλέξετε ότι άλλο θέλετε αλλά αυτό εφαρμόζει καλύτερα στο πρόγραμμα Visual Studio Code).  

[![C/C++](/post_images/vscode/search-cpp-extension.png "C/C++")](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

Δείτε περισσότερα στην [τεκμηρίωση στην σελίδα του Visual Studio Code](https://code.visualstudio.com/docs/languages/cpp).  

### ΕΓΚΑΤΑΣΤΑΣΗ για Java

Για την Java χρειάζεται το πρόσθετο που μπορείτε να βρείτε στο [Market place](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).  

Ανοίξτε το VS Code Quick Open (με τα πλήκτρα Ctrl+P) και επικολήστε την εντολή:  

{% highlight ruby %}
ext install vscjava.vscode-java-pack
{% endhighlight %} 

To Extension Pack for Java, περιέχει τα παρακάτω:  

*   📦 [Language Support for Java™ by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.java)  
*   📦 [Debugger for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)  
*   📦 [Java Test Runner](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-test)  
*   📦 [Maven for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-maven)  
*   📦 [Project Manager for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-dependency)  
*   📦 [Visual Studio IntelliCode](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode)  

Δείτε περισσότερα στην [τεκμηρίωση στην σελίδα του Visual Studio Code](https://code.visualstudio.com/docs/languages/java).  

### Μεταφορά extensions

Αφού έγινε η εγκατάσταση, τώρα έπρεπε να μεταφέρω και τα extensions που είχα στην προηγούμενη εγκατάσταση (ή αν θέλετε να συγχρονίσετε δυο συστήματα να έχουν τα ίδια extensions). Υπάρχουν 2-3 τρόποι. Θα γράψω κάποιους.  
Καταρχήν ανοίξτε το VSCode και ανοίξτε το τερματικό. Εκεί δώστε την παρακάτω εντολή:  

{% highlight ruby %}
code --list-extensions | xargs -L 1 echo code --install-extension
{% endhighlight %} 

Εμένα μου έβγαλε την παρακάτω λίστα.  

{% highlight ruby %}
code --install-extension formulahendry.code-runner  
code --install-extension ms-python.python  
code --install-extension ms-python.vscode-pylance  
code --install-extension ms-toolsai.jupyter  
code --install-extension ms-vscode.cmake-tools  
code --install-extension ms-vscode.cpptools  
code --install-extension redhat.java  
code --install-extension twxs.cmake  
code --install-extension VisualStudioExptTeam.vscodeintellicode  
code --install-extension vscjava.vscode-java-debug  
code --install-extension vscjava.vscode-java-dependency  
code --install-extension vscjava.vscode-java-pack  
code --install-extension vscjava.vscode-java-test  
code --install-extension vscjava.vscode-maven  
{% endhighlight %} 

Την αντέγραψα σε ένα αρχείο txt και μετά την νέα εγκατάσταση, εκτέλεσα μια προς μια τις εντολές.  

Εναλλακτικά υπάρχουν και πιο αυτοματοποιημένοι τρόποι. Δηλαδή θα φτιάξετε ένα αρχείο με τα extensions που έχετε εγκατεστημένα με την παρακάτω εντολή:  

{% highlight ruby %}
code --list-extensions > vscode-extensions.list
{% endhighlight %} 

Μεταφέρετε το αρχείο **vscode-extensions.list** στο νέο σύστημα και μετά με την παρακάτω εντολή θα εγκατασταθούν.  

{% highlight ruby %}
cat vscode-extensions.list | xargs -L 1 code --install-extension
{% endhighlight %} 

### Διάφορες ρυθμίσεις

Μια ρύθμιση που την είχα κάνει στην εγκατάσταση με flatpak και δεν θυμόμουν ήταν αυτή με την οποία όταν εκτελούσα ένα πρόγραμμα σε C, άνοιγε το ενσωματωμένο τερματικό και εκεί έβλεπα τα αποτελέσματα. Βρήκα το [How to run a C program in Visual Studio Code?](https://www.javatpoint.com/how-to-run-a-c-program-in-visual-studio-code) όπου μας λέει όλα αυτά που ανέφερα παραπάνω (με την προσθήκη πως γίνεται και σε Windows). Σε ένα σημείο λέει ότι όταν εκτελείται ο κώδικας, τότε έχουμε ως έξοδο ένα Read-Only αποτέλεσμα χωρίς τη δυνατότητα να εισάγει ο χρήστης δεδομένα.  

![Read-Only Output](/post_images/vscode/how-to-run-a-c-program-in-visual-studio-code33.png "Read-Only Output")

Οπότε λέει να προβούμε στην παρακάτω ρύθμιση.  

* Ανοίγουμε **Αρχείο>Επιλογές>Ρυθμίσεις (File>Preferences>Settings)**

![File>Preferences>Settings](/post_images/vscode/how-to-run-a-c-program-in-visual-studio-code34.png "File>Preferences>Settings")


* Και αφού ανοίξει, κάντε κλικ στο **Extensions** για να καθορίσουμε τις ρυθμίσεις για τον C Compiler.  

![Extensions](/post_images/vscode/how-to-run-a-c-program-in-visual-studio-code35.png "Extensions")

* Μετακινηθείτε προς τα κάτω μέχρι να βρείτε το **Run Code Configuration**.  

![Run Code Configuration](/post_images/vscode/how-to-run-a-c-program-in-visual-studio-code36.png "Run Code Configuration")

* Μετακινηθείτε προς τα κάτω και κάντε κλικ στην επιλογή **Run In Terminal**.  

![Run In Terminal](/post_images/vscode/how-to-run-a-c-program-in-visual-studio-code37.png "Run In Terminal")

Κλείστε και ανοίξτε ξανά το πρόγραμμα για να πάρει τις ρυθμίσεις. Τώρα όταν εκτελείτε ένα [πρόγραμμα C](https://www.scaler.com/topics/c/first-c-program/), θα υπάρχει αλληλεπόδραση χρήστη με τον υπολογιστή.  

Ελπίζω να ήταν πολύ βοηθητικές οι οδηγίες. Μοιραστείτε με τους φίλους σας που αντιμετωπίζουν πρόβλημα εγκατάστασης ή ρυθμίσεων.  

Πηγές:  
[Visual Studio Code on Linux](https://code.visualstudio.com/docs/setup/linux)

Αρχική δημοσίευση:  
[https://eiosifidis.blogspot.com/2021/08/visual-studio-code.html](https://eiosifidis.blogspot.com/2021/08/visual-studio-code.html)
