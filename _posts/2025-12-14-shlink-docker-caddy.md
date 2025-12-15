---
layout: post
title: "Shlink Docker Deployment με Caddy Reverse Proxy"
date: 2025-12-15 12:00:00
description: Αυτός ο οδηγός περιγράφει την εγκατάσταση του Shlink χρησιμοποιώντας Docker Compose, με Reverse Proxy τον Caddy. Η εγκατάσταση χρησιμοποιεί Bind Mounts για εύκολη μεταφορά (portability) και backup.
tags:
  - shlink
  - docker
  - caddy
  - url shortener
categories:
  - Greek
  - DOCKER
  - CADDY
  - SHLINK
  - TUTORIALS
twitter_text: "Shlink Docker Deployment με Caddy Reverse Proxy"
---

## 1. Περιγραφή Συστήματος
*   **Υπηρεσία:** Shlink (Self-hosted URL Shortener)
*   **Μέθοδος:** Docker Compose
*   **Βάση Δεδομένων:** SQLite (Embedded - για ελαχιστοποίηση πόρων)
*   **Reverse Proxy:** Caddy (μέσω εξωτερικού Docker network `web`)
*   **Storage:** Local Bind Mounts (για εύκολο backup/migration)
*   **Domain:** `mytiny.duckdns.org`

## 2. Δομή Φακέλου
Δημιουργία φακέλου εγκατάστασης:
```bash
/docker/shlink/
├── docker-compose.yml
└── database/          (Φάκελος για το αρχείο SQLite)
```

## 3. Αρχεία Ρυθμίσεων

### Α. `docker-compose.yml`
Δημιουργήστε το αρχείο στον φάκελο του project:

```yaml
services:
  shlink:
    image: shlinkio/shlink:stable
    container_name: shlink
    restart: unless-stopped
    environment:
      # --- General Settings ---
      - DEFAULT_DOMAIN=mytiny.duckdns.org
      - IS_HTTPS_ENABLED=true
      - SHELL_VERBOSITY=0
      
      # --- Database Settings (SQLite) ---
      # Χρήση SQLite για εξοικονόμηση πόρων (RAM/CPU)
      - DB_DRIVER=sqlite
      # Το αρχείο θα δημιουργηθεί στο /etc/shlink/data/database.sqlite
      
      # --- URL Shortening Config ---
      - SHORT_DOMAIN_SCHEMA=https
      # Validate URLs to avoid shortening broken links
      - VALIDATE_URL=true 

    volumes:
      # Bind mount για εύκολο backup. 
      # Αντιστοιχίζει τον τοπικό φάκελο database στον εσωτερικό φάκελο data
      - ./database:/etc/shlink/data

    networks:
      - web

networks:
  web:
    external: true
```

### Β. `Caddyfile` (Τμήμα ρύθμισης)
Προσθήκη στο κεντρικό αρχείο ρυθμίσεων του Caddy:

```caddy
mytiny.duckdns.org {
    # Reverse proxy προς το container 'shlink' στην εσωτερική θύρα 8080
    reverse_proxy shlink:8080
    
    # Security Headers (Best Practices)
    header {
        Strict-Transport-Security "max-age=31536000;"
        X-XSS-Protection "1; mode=block"
        X-Content-Type-Options "nosniff"
        X-Frame-Options "DENY"
        Referrer-Policy "no-referrer-when-downgrade"
    }
}
```

## 4. Διαδικασία Εγκατάστασης & Ρύθμιση Δικαιωμάτων

Το Shlink τρέχει εσωτερικά με χρήστη **UID 1001**. Για να αποφύγουμε σφάλματα εγγραφής (Permission Denied) στην SQLite βάση, πρέπει να προετοιμάσουμε τον φάκελο με τον σωστό ιδιοκτήτη.

Εκτελέστε τις παρακάτω εντολές στο τερματικό:

```bash
# 1. Μετάβαση στον φάκελο
cd /docker/shlink

# 2. Δημιουργία του φακέλου για τη βάση δεδομένων
mkdir database

# 3. Ρύθμιση ιδιοκτησίας (Chown)
# Δίνουμε τον φάκελο στον χρήστη 1001 (που χρησιμοποιεί το container)
sudo chown -R 1001:1001 ./database

# 4. Εκκίνηση υπηρεσίας
docker compose up -d
```

## 5. Initial Setup (Δημιουργία API Key)
Μετά την εκκίνηση, το Shlink δεν έχει GUI από προεπιλογή και χρειάζεται ένα API Key για να συνδεθείτε (π.χ. μέσω web client ή CLI).

Εκτελέστε:
```bash
docker exec -it shlink shlink api-key:generate
```
*Αντιγράψτε το κλειδί που θα εμφανιστεί. Θα χρειαστεί για τη διαχείριση.*

---

## 6. Διαδικασία Backup & Restore (Migration)

Επειδή χρησιμοποιούμε **Bind Mounts** και **SQLite**, το backup είναι απλά μια συμπίεση του φακέλου.

### Backup (Στο τρέχον μηχάνημα)
```bash
cd /docker
# Συμπίεση όλου του φακέλου shlink
tar -czvf shlink-backup.tar.gz shlink/
```

### Restore (Σε νέο μηχάνημα)
1. Μεταφορά του `shlink-backup.tar.gz` στο νέο μηχάνημα.
2. Αποσυμπίεση:
   ```bash
   tar -xzvf shlink-backup.tar.gz
   ```
3. **Σημαντικό:** Επιβεβαίωση δικαιωμάτων στο νέο μηχάνημα (αν χάθηκαν κατά τη μεταφορά):
   ```bash
   cd shlink
   sudo chown -R 1001:1001 ./database
   ```
4. Εκκίνηση:
   ```bash
   docker compose up -d
   ```
