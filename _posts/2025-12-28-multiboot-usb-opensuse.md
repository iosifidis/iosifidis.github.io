---
layout: post
title: "Κατασκευή Multiboot openSUSE USB (UEFI & Agama Ready)"
date: 2025-12-28 12:00:00
description: Κατασκευή ενός multiboot usb με openSUSE όταν το Ventoy μας απογοητεύει
tags:
  - multiboot
  - openSUSE
  - Leap
  - Tumbleweed
  - Agama
categories:
  - Greek
  - OPENSUSE
  - LEAP
  - TUBLEWEED
  - MULTIBOOT
twitter_text: "Κατασκευή Multiboot openSUSE USB (UEFI & Agama Ready)"
---

![Κατασκευή Multiboot openSUSE USB](/post_images/opensuse/opensuse_usb.jpg "Κατασκευή Multiboot openSUSE USB"){:width="320px"}

Αυτός ο οδηγός περιγράφει την δημιουργία ενός multiboot usb με openSUSE. Υπάρχει το [Ventoy](https://www.ventoy.net/) το οποίο δουλεύει καλά σε γενικές γραμμές. Όμως όταν πάμε σε openSUSE, υπάρχει πρόβλημα. Στο [wiki](https://en.opensuse.org/Create_installation_USB_stick#Ventoy) αναφερέρεται ότι το πρόβλημα προκύπτει από την προσθήκη  `rdinit=/vtoy/vtoy` μάλλον μετά την εγκατάσταση. Αν το αφερέσετε μπορεί να διορώσει το πρόβλημα. Εδώ θα δούμε έναν διαφορετικό τρόπο που δουλεύει μια χαρά.

## 1. Στρατηγική Κατασκευής
*   **Wipe:** Καθαρισμός του δίσκου από "φαντάσματα" παλιών ISO.
*   **Partitioning (GPT):** 
    *   `sdb2` (FAT32 - Label: `GRUB_BOOT`): Περιέχει τον bootloader και το θέμα.
    *   `sdb3` (Ext4 - Label: `MULTIBOOT`): Περιέχει τα αρχεία ISO.
*   **Filesystem:** Επιλέχθηκε το **Ext4** για την αποθήκευση των ISO καθώς είναι το πιο συμβατό με τον GRUB για multiboot χρήση.

---

## 2. Βήματα Υλοποίησης (Τερματικό Ubuntu 24.04)

Βρίσκουμε το usb με οποιονδήποτε τρόπο γνωρίζουμε. Ας πούμε με την εντολή `lsblk` όπου βλέπουμε το usb μας. Έστω εδώ είναι `sdb`.

### Α. Καθαρισμός και Partitions
{% highlight ruby %}
# 1. Καθαρισμός (ΠΡΟΣΟΧΗ στο sdb)
sudo dd if=/dev/zero of=/dev/sdb bs=1M count=100
sudo sync

# 2. Δημιουργία GPT (μέσω gdisk)
1.  `sudo gdisk /dev/sdb`
2.  `o` -> `y` (Νέο GPT)
3.  `n` -> `1` (Enter) -> `+1M` -> `EF02` (BIOS Boot)
4.  `n` -> `2` (Enter) -> `+500M` -> `EF00` (EFI System - FAT32)
5.  `n` -> `3` (Enter) -> (Enter) -> `8300` (Linux Data - Ext4)
6.  `w` -> `y` (Αποθήκευση)
{% endhighlight %}

### Β. Format και Εγκατάσταση

{% highlight ruby %}
# 1. Formatting
sudo mkfs.fat -F32 -n "GRUB_BOOT" /dev/sdb2
sudo mkfs.ext4 -L "MULTIBOOT" /dev/sdb3

# 2. Mount και GRUB Install
sudo mkdir -p /mnt/boot_usb /mnt/data_usb
sudo mount /dev/sdb2 /mnt/boot_usb
sudo grub-install --target=x86_64-efi --removable --recheck --efi-directory=/mnt/boot_usb --boot-directory=/mnt/boot_usb/boot /dev/sdb

# 3. Δομή Φακέλων
sudo mount /dev/sdb3 /mnt/data_usb
sudo mkdir -p /mnt/data_usb/iso
{% endhighlight %}

---

## 3. Το Πλήρες Αρχείο `grub.cfg`
Τοποθέτησε το παρακάτω στο `/mnt/boot_usb/boot/grub/grub.cfg` (sudo nano /mnt/boot_usb/boot/grub/grub.cfg):

{% highlight ruby %}
# --- Φόρτωση Modules ---
insmod part_gpt
insmod ext2
insmod fat
insmod efi_gop
insmod video_fb
insmod gfxterm
insmod png
insmod search_label
insmod loopback
insmod linux

# --- Ρυθμίσεις Γραφικών & Theme ---
set gfxmode=auto
terminal_output gfxterm
set theme=/boot/grub/themes/openSUSE/theme.txt
export theme

# --- Αναζήτηση Κατάτμησης MULTIBOOT ---
search --no-floppy --set=isopart --label MULTIBOOT

set isopath=/iso
set default=0
set timeout=10

# --- MENU ENTRIES ---

menuentry "--- openSUSE TUMBLEWEED ---" { true }

menuentry "Tumbleweed GNOME Live" --class opensuse {
    set isofile="${isopath}/openSUSE-Tumbleweed-GNOME-Live-x86_64-Current.iso"
    loopback loop ($isopart)$isofile
    echo "Loading Linux kernel..."
    linux (loop)/boot/x86_64/loader/linux iso-scan/filename=$isofile root=live:CDLABEL=openSUSE_Tumbleweed_GNOME_Live splash=silent quiet showopts
    echo "Loading initial ramdisk..."
    initrd (loop)/boot/x86_64/loader/initrd
}

menuentry "Tumbleweed NET Installer" --class opensuse {
    set isofile="${isopath}/openSUSE-Tumbleweed-NET-x86_64-Current.iso"
    loopback loop ($isopart)$isofile
    linux (loop)/boot/x86_64/loader/linux install=hd:LABEL=MULTIBOOT:$isofile splash=silent quiet
    initrd (loop)/boot/x86_64/loader/initrd
}

menuentry "Tumbleweed Full DVD Installer" --class opensuse {
    set isofile="${isopath}/openSUSE-Tumbleweed-DVD-x86_64-Current.iso"
    loopback loop ($isopart)$isofile
    linux (loop)/boot/x86_64/loader/linux install=hd:LABEL=MULTIBOOT:$isofile splash=silent quiet
    initrd (loop)/boot/x86_64/loader/initrd
}

menuentry "--- openSUSE LEAP 16 (Agama) ---" { true }

menuentry "Leap 16 Standard Install" --class opensuse {
    set isofile="${isopath}/Leap-16.0-DVD-x86_64.iso"
    loopback loop ($isopart)$isofile
    linux (loop)/boot/x86_64/loader/linux install=hd:LABEL=MULTIBOOT:$isofile agama.run rd.live.image iso-scan/filename=$isofile nomodeset video=1024x768 splash=silent quiet
    initrd (loop)/boot/x86_64/loader/initrd
}

menuentry "Leap 16 OEM Install (Self-Install)" --class opensuse {
    set isofile="${isopath}/Leap-16.0-OEM-x86_64.iso"
    loopback loop ($isopart)$isofile
    linux (loop)/boot/x86_64/loader/linux install=hd:LABEL=MULTIBOOT:$isofile agama.oem=1 rd.live.image iso-scan/filename=$isofile nomodeset video=1024x768 splash=silent quiet
    initrd (loop)/boot/x86_64/loader/initrd
}

menuentry "Leap 16 NET Installer" --class opensuse {
    set isofile="${isopath}/Leap-16.0-NET-x86_64.iso"
    loopback loop ($isopart)$isofile
    linux (loop)/boot/x86_64/loader/linux install=hd:LABEL=MULTIBOOT:$isofile agama.run rd.live.image iso-scan/filename=$isofile nomodeset video=1024x768 splash=silent quiet
    initrd (loop)/boot/x86_64/loader/initrd
}

menuentry "--- SYSTEM ---" { true }
menuentry "Reboot" { reboot }
menuentry "Power Off" { halt }
{% endhighlight %}

Μαζί με το παραπάνω `grub.cfg` πρέπει να βάλουμε και το θέμα (για την αισθητική).

1. Κατεβάστε το αρχείο [grub_themes.zip](/post_images/opensuse/grub_themes.zip)

2. Αποσυμπιέστε το στον φάκελο

{% highlight ruby %}
sudo unzip grub_themes.zip -d /mnt/boot_usb/boot/grub/
{% endhighlight %}

---

## 4. Αντιγραφή αρχείων ISO στο USB

Αντιγράφουμε τα ISO της διανομής μέσα στον φάκελο του USB προσέχοντας να έχουν το ίδιο όνομα που έχουμε στο `grub.cfg`.

{% highlight ruby %}
sudo cp *.ISO /mnt/data_usb/iso
sudo sync
{% endhighlight %}

---

## 5. Δοκιμή σε Εικονικό Περιβάλλον (QEMU)
Για να λειτουργήσει σωστά ο Agama installer στο QEMU, απαιτούνται 4GB RAM και ο virtio οδηγός γραφικών:

{% highlight ruby %}
sudo qemu-system-x86_64 -m 4G -enable-kvm -cpu host \
-bios /usr/share/ovmf/OVMF.fd \
-drive file=/dev/sdb,format=raw,index=0,media=disk \
-vga virtio
{% endhighlight %}

---

## 5. Κρίσιμες Παρατηρήσεις
1.  **`install=hd:LABEL=MULTIBOOT:$isofile`**: Αυτή η παράμετρος είναι απαραίτητη για να βρει ο installer το ISO μέσα σε ένα Multiboot USB.
2.  **`rd.live.image iso-scan/filename=$isofile`**: Αναγκάζει το σύστημα να ψάξει για το αρχείο ISO και να το προσαρτήσει ως Live Image, διορθώνοντας το σφάλμα `dracut-initqueue timeout`.
3.  **`nomodeset video=1024x768`**: Χρησιμοποιείται μόνο για τη σταθερότητα της εικόνας μέσα στο QEMU. Σε πραγματικό υπολογιστή, αυτές οι δύο παράμετροι μπορούν να αφαιρεθούν για καλύτερη απόδοση γραφικών.
4.  **Συγχρονισμός:** Πάντα εκτελούμε `sync` πριν αποσυνδέσουμε το USB για να βεβαιωθούμε ότι τα μεγάλα αρχεία ISO γράφτηκαν πλήρως.

Με αυτό το configuration, το Live USB σου είναι έτοιμο για χρήση σε οποιοδήποτε σύγχρονο UEFI μηχάνημα!
