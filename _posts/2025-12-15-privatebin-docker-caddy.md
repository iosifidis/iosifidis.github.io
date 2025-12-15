---
layout: post
title: "PrivateBin Docker Deployment με Caddy Reverse Proxy"
date: 2025-12-15 12:00:00
description: Αυτός ο οδηγός περιγράφει την εγκατάσταση του PrivateBin χρησιμοποιώντας Docker Compose, με Reverse Proxy τον Caddy. Η εγκατάσταση χρησιμοποιεί Bind Mounts για εύκολη μεταφορά (portability) και backup.
tags:
  - pricatebin
  - docker
  - caddy
categories:
  - Greek
  - DOCKER
  - CADDY
  - PRIVATEBIN
  - TUTORIALS
twitter_text: "PrivateBin Docker Deployment με Caddy Reverse Proxy"
---

Αυτός ο οδηγός περιγράφει την εγκατάσταση του **PrivateBin** χρησιμοποιώντας Docker Compose, με Reverse Proxy τον **Caddy**. Η εγκατάσταση χρησιμοποιεί **Bind Mounts** για εύκολη μεταφορά (portability) και backup.

### Προαπαιτούμενα
1.  Docker & Docker Compose εγκατεστημένα.
2.  Υπάρχουσα εγκατάσταση Caddy που τρέχει στο δίκτυο `web`.
3.  Domain name (π.χ. μέσω DuckDNS) ρυθμισμένο να δείχνει στην IP του server.

---

### 1. Δομή Φακέλων
Δημιουργούμε έναν φάκελο (π.χ. `/docker/privatebin`) με την εξής δομή:

```text
/privatebin
├── docker-compose.yml   # Το αρχείο ορισμού του container
├── conf.php             # Οι ρυθμίσεις της εφαρμογής
└── data/                # Φάκελος όπου αποθηκεύονται τα pastes (δημιουργείται αυτόματα)
```

---

### 2. Αρχείο `docker-compose.yml`
Δημιουργούμε το αρχείο `docker-compose.yml`. Χρησιμοποιούμε την εικόνα `nginx-fpm-alpine` που περιέχει ενσωματωμένο web server.

```yaml
services:
  privatebin:
    image: privatebin/nginx-fpm-alpine
    container_name: privatebin
    restart: unless-stopped
    
    # Τρέχει ως χρήστης 'nobody' (uid:65534) για ασφάλεια
    read_only: true
    
    # Η εικόνα από προεπιλογή ακούει στην πόρτα 8080
#    ports:
#      - "8090:8080"  # <--- ΠΡΟΣΘΕΣΕ ΑΥΤΟ

    # Χρήση Bind Mounts για εύκολο backup/restore
    volumes:
      - ./data:/srv/data
      - ./conf.php:/srv/conf.php:ro
    networks:
      - web
    # Ο χρήστης μέσα στο container είναι ο 'nobody' (uid: 65534)
    
networks:
  web:
    external: true
```

---

### 3. Αρχείο Ρυθμίσεων `conf.php`
Δημιουργούμε το αρχείο `conf.php`.
**ΠΡΟΣΟΧΗ:** Η παράμετρος `basepath` είναι κρίσιμη για να λειτουργεί πίσω από Reverse Proxy.

```php
; <?php exit; ?> DO NOT REMOVE THIS LINE
[main]
name = "My PrivateBin"

; Αντικαταστήστε με το δικό σας domain. Πρέπει να τελειώνει με /
basepath = "https://privatebin.duckdns.org/"

; Ρυθμίσεις συζήτησης
discussion = true
opendiscussion = false

; Ενεργοποίηση Password
password = true

; Ρυθμίσεις λήξης αρχείων
[expire]
default = "1week"

[expire_options]
5min = 300
10min = 600
1hour = 3600
1day = 86400
1week = 604800
1month = 2592000
1year = 31536000
never = 0

[traffic]
limit = 10
```

---

### 4. Ρύθμιση Δικαιωμάτων (Permissions)
Το PrivateBin για λόγους ασφαλείας τρέχει ως χρήστης `nobody` (UID: 65534). Πρέπει ο φάκελος `data` στον host να ανήκει σε αυτόν τον χρήστη, αλλιώς η εφαρμογή δεν θα μπορεί να αποθηκεύσει τίποτα.

Τρέξτε τις παρακάτω εντολές στον φάκελο του project:

```bash
# Δημιουργία φακέλου (αν δεν υπάρχει)
mkdir data

# Αλλαγή ιδιοκτησίας
sudo chown -R 65534:65534 ./data

# Το conf.php αρκεί να είναι αναγνώσιμο
sudo chmod 644 conf.php
```

---

### 5. Ρύθμιση Caddy (Caddyfile)
Στο κεντρικό αρχείο `Caddyfile` του συστήματος, προσθέτουμε το παρακάτω μπλοκ.
Οι headers `X-Forwarded-Proto` και `X-Forwarded-Ssl` είναι απαραίτητοι για να καταλάβει το PrivateBin ότι λειτουργεί με HTTPS (ώστε να ενεργοποιηθεί η κρυπτογράφηση στον browser).

```caddy
privatebin.duckdns.org {
    # Προαιρετικό logging
    log {
        output file /var/log/caddy/privatebin.log
    }

    reverse_proxy privatebin:8080 {
        header_up Host {host}
        header_up X-Real-IP {remote}
        # Απαραίτητα για να λειτουργεί η κρυπτογράφηση (WebCrypto API)
        header_up X-Forwarded-Proto https
        header_up X-Forwarded-Ssl on
    }
}
```

*Μετά την αλλαγή, κάντε reload ή restart το Caddy container.*

---

### 6. Εκκίνηση και Έλεγχος

1.  Ξεκινάμε το PrivateBin:
    ```bash
    docker compose up -d
    ```
2.  Επανεκκινούμε το Caddy (για να δει το νέο config):
    ```bash
    docker restart caddy
    ```
3.  Ανοίγουμε τον browser στο `https://privatebin.duckdns.org`.
    *Tip: Αν δείτε παλιά έκδοση ή σφάλματα, πατήστε `CTRL + F5` για καθαρισμό cache.*

---

### 7. Διαδικασία Backup & Restore (Μεταφορά)

Επειδή χρησιμοποιήσαμε **bind mounts**, η μεταφορά σε νέο μηχάνημα είναι εξαιρετικά απλή.

**Backup (Στο παλιό μηχάνημα):**
1.  Σταματάμε το container: `docker compose down`
2.  Συμπιέζουμε όλο τον φάκελο:
    ```bash
    tar -czvf privatebin-backup.tar.gz .
    ```

**Restore (Στο νέο μηχάνημα):**
1.  Μεταφέρουμε και αποσυμπιέζουμε το αρχείο.
    ```bash
    tar -xzvf privatebin-backup.tar.gz
    ```
2.  **Σημαντικό:** Ελέγχουμε αν διατηρήθηκαν τα δικαιώματα στον φάκελο `data`.
    ```bash
    ls -l data
    # Αν δεν λέει nobody/nogroup ή 65534, τρέχουμε πάλι:
    sudo chown -R 65534:65534 ./data
    ```
3.  Ξεκινάμε: `docker compose up -d`
