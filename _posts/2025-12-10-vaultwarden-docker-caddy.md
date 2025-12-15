---
layout: post
title: "Vaultwarden Docker Deployment με Caddy Reverse Proxy"
date: 2025-12-10 12:00:00
description: Αυτός ο οδηγός περιγράφει την εγκατάσταση του Vaultwarden χρησιμοποιώντας Docker Compose, με Reverse Proxy τον Caddy. Η εγκατάσταση χρησιμοποιεί Bind Mounts για εύκολη μεταφορά (portability) και backup.
tags:
  - vaultwarden
  - docker
  - caddy
  - url shortener
categories:
  - Greek
  - DOCKER
  - CADDY
  - VAULTWARDEN
  - TUTORIALS
twitter_text: "Vaultwarden Docker Deployment με Caddy Reverse Proxy"
---

Για το Raspberry Pi, η καλύτερη και πιο ελαφριά έκδοση του Bitwarden είναι το **Vaultwarden** (πρώην bitwarden_rs). Είναι γραμμένο σε Rust, τρέχει άψογα σε Docker και είναι απόλυτα συμβατό με όλες τις επίσημες εφαρμογές (apps) και browser extensions του Bitwarden.

Ακολουθούν τα βήματα για την εγκατάσταση, τη ρύθμιση του Caddy και τη διαδικασία backup/μεταφοράς.

### Βήμα 1: Το αρχείο `docker-compose.yml`

Δημιούργησε έναν φάκελο (π.χ. `vaultwarden`) και μέσα σε αυτόν το αρχείο `docker-compose.yml`.

Χρησιμοποιούμε έναν τοπικό φάκελο (`./vw-data`) για τα δεδομένα, ώστε να είναι εύκολο το backup.

```yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    volumes:
      - ./vw-data:/data  # Εδώ αποθηκεύεται η βάση δεδομένων
    environment:
      - WEBSOCKET_ENABLED=true  # Για άμεσο συγχρονισμό συσκευών
      - SIGNUPS_ALLOWED=true    # Θα το αλλάξουμε σε false αφού κάνεις εγγραφή
      - DOMAIN=https://myvault.duckdns.org
    networks:
      - web

networks:
  web:
    external: true
```

*Σημείωση: Υποθέτω ότι το δίκτυο που έχεις ήδη φτιάξει στο Docker ονομάζεται `web`. Αν έχει άλλο όνομα, άλλαξέ το στο τέλος του αρχείου.*

### Βήμα 2: Ρύθμιση του Caddyfile

Πρόσθεσε το παρακάτω μπλοκ στο υπάρχον `Caddyfile` σου. Επειδή είσαι στο ίδιο docker network (`web`), το Caddy μπορεί να "δει" το container με το όνομα `vaultwarden`.

```caddy
myvault.duckdns.org {
    # Προσθήκη ασφάλειας HSTS κλπ (προαιρετικά αλλά καλό)
    header {
        Strict-Transport-Security "max-age=31536000;"
        X-XSS-Protection "1; mode=block"
        X-Frame-Options "DENY"
    }

    # Reverse proxy στο Vaultwarden (Port 80 εσωτερικά)
    reverse_proxy vaultwarden:80 {
       # Ρυθμίσεις για να δουλεύουν τα WebSockets
       header_up X-Real-IP {remote_host}
    }
}
```

Κάνε reload το Caddy (π.χ. `docker reload caddy` ή απλά restart το container του Caddy).

### Βήμα 3: Εκκίνηση και Πρώτη Εγγραφή

1.  Τρέξε το Vaultwarden:
    ```bash
    docker compose up -d
    ```
2.  Μπες στο `https://myvault.duckdns.org`.
3.  Φτιάξε τον λογαριασμό σου (Create Account).
4.  **Σημαντικό:** Αφού φτιάξεις τον λογαριασμό σου, πήγαινε πίσω στο `docker-compose.yml` και άλλαξε το:
    `SIGNUPS_ALLOWED=false`
5.  Κάνε επανεκκίνηση για να κλειδώσεις τις εγγραφές (ώστε να μην μπορεί να γραφτεί κανένας ξένος):
    ```bash
    docker compose up -d
    
    ή
    
    docker compose up -d --force-recreate
    ```

---

### Βήμα 4: Backup και Μεταφορά σε VM (Migration)

Ο πιο εύκολος τρόπος να πάρεις τα πάντα και να τα πας σε ένα VM είναι να αντιγράψεις τον φάκελο `vw-data`.

**Διαδικασία στο Raspberry Pi (Source):**

1.  Σταμάτα το container για να μην γίνονται εγγραφές στη βάση εκείνη τη στιγμή (σημαντικό για να μη διαφθαρεί η βάση `db.sqlite3`):
    ```bash
    docker stop vaultwarden
    ```
2.  Συμπίεσε τον φάκελο δεδομένων:
    ```bash
    tar -czvf vault-backup.tar.gz ./vw-data
    ```
3.  Στείλε το αρχείο στο VM (αν έχεις Linux VM με SSH):
    ```bash
    scp vault-backup.tar.gz user@IP-TOU-VM:/home/user/
    ```

**Διαδικασία στο VM (Destination):**

1.  Στο VM, φτιάξε έναν φάκελο και βάλε μέσα το ίδιο `docker-compose.yml` που είχες στο Raspberry.
2.  Αποσυμπίεσε το backup στον ίδιο φάκελο:
    ```bash
    tar -xzvf vault-backup.tar.gz
    ```
    *(Βεβαιώσου ότι ο φάκελος `vw-data` βρίσκεται δίπλα στο `docker-compose.yml` όπως ήταν και πριν).*
3.  Ξεκίνα το container:
    ```bash
    docker compose up -d
    ```

Έτσι, θα έχεις ακριβώς τους ίδιους κωδικούς, ρυθμίσεις και κλειδιά κρυπτογράφησης στο νέο μηχάνημα.

---

Είναι απολύτως φυσιολογικό να φαίνεται το κουμπί. Το Vaultwarden είναι αυτό που λέμε "Single Page Application", οπότε το "πρόσωπο" της ιστοσελίδας φορτώνει ολόκληρο, αλλά ο "εγκέφαλος" από πίσω (το API) κόβει την πρόσβαση όταν πας να κάνεις την ενέργεια.

Τώρα που είσαι έτοιμος, σου δίνω **3 σημαντικές συμβουλές** για τη συνέχεια, για να έχεις το κεφάλι σου ήσυχο:

### 1. Ενεργοποίηση 2FA (Δύο Βημάτων)
Πριν αρχίσεις να αποθηκεύεις κωδικούς, πήγαινε στις ρυθμίσεις του λογαριασμού σου (μέσα στο web vault) -> **Security** -> **Two-step Login**.
Ενεργοποίησε τουλάχιστον ένα Authenticator App (Google Authenticator, Authy, Aegis, κλπ).
*Γιατί;* Επειδή το vault σου είναι πλέον στο ίντερνετ. Αν κάποιος μαντέψει τον κωδικό σου, το 2FA θα τον σταματήσει.

### 2. Πώς κάνεις Update
Επειδή είναι θέμα ασφαλείας, καλό είναι να αναβαθμίζεις το Vaultwarden συχνά. Η διαδικασία είναι πολύ απλή:

Μπαίνεις στον φάκελο και τρέχεις:
```bash
docker-compose pull       # Κατεβάζει τη νέα έκδοση
docker-compose up -d      # Επανεκκινεί το container με τη νέα έκδοση
docker image prune -f     # (Προαιρετικά) Σβήνει την παλιά έκδοση για να γλιτώσεις χώρο
```

### 3. Αυτοματοποίηση Backup (Το πιο σημαντικό!)
Είπαμε πριν πώς να το πάρεις χειροκίνητα. Επειδή όμως μπορεί να το ξεχάσεις, σου προτείνω να βάλεις μια υπενθύμιση ή ένα απλό script (cronjob) στο Raspberry που να κάνει copy τον φάκελο `vw-data` κάπου αλλού (π.χ. σε ένα USB στικ ή σε ένα cloud storage) μια φορά την εβδομάδα.

Αν χάσεις τον φάκελο `vw-data`, χάνεις τους κωδικούς σου. Οπότε πρόσεχέ τον σαν τα μάτια σου!
