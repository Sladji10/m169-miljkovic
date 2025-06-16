# Private Cloud mit Nextcloud, pgAdmin & Mailhog auf AWS EC2 mit Docker

## Projektübersicht

**Ziel ist es, eine vollständige private Cloud-Umgebung auf einer AWS EC2-Instanz zu betreiben, in der folgende Dienste containerisiert laufen:**

- **Nextcloud** (Cloud-Speicherlösung)
- **PostgreSQL** (Datenbank)
- **pgAdmin 4** (Web-Interface zur Verwaltung von PostgreSQL)
- **Mailhog** (lokaler SMTP-Mailserver zum Testen)

Alle Dienste sollen in einem Docker-Netzwerk verbunden werden. Der Zugang erfolgt über das Webinterface, Konfiguration über Docker Compose. Alle Schritte werden einzeln dokumentiert.

## 🟢 1. AWS EC2 Instanz erstellen

- Image: Ubuntu 24.04 LTS
- Typ: t2.medium
- Speichermenge: mind. 25 GB (wichtig!)
- Vorhandenes Key-Pair verwenden: **sladjan.pem**
- Sicherheitsgruppe: Ports = **22, 8080, 5050, 8025, 1025** freigeben
- ***Instanz starten***
- Über SSH verbinden:
  - ```bash ssh -i C:\Users\sladjan.miljkovic\m169\sladjan.pem ubuntu@172.31.92.16```

## 🟢 2. Docker & Docker Compose installieren

```yaml
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
```

## 🟢 3. Projektverzeichnis anlegen

```yaml
mkdir LB2 && cd LB2
```

## 🟢 4. docker-compose.yml erstellen

```yaml
version: '3.8'

services:
  db:
    image: postgres:latest
    restart: always
    environment:
      POSTGRES_DB: nextcloud
      POSTGRES_USER: nc_user
      POSTGRES_PASSWORD: nc_pass
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - cloudnet

  nextcloud:
    build: ./nextcloud-custom
    restart: always
    ports:
      - "8080:80"
    environment:
      - POSTGRES_DB=nextcloud
      - POSTGRES_USER=nc_user
      - POSTGRES_PASSWORD=nc_pass
      - POSTGRES_HOST=db    
    volumes:
      - nextcloud_data:/var/www/html
    depends_on:
      - db
    networks:
      - cloudnet

  pgadmin:
    image: dpage/pgadmin4
    restart: always
    ports:
      - "5050:80"
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@admin.com
      PGADMIN_DEFAULT_PASSWORD: admin123
    depends_on:
      - db
    networks:
      - cloudnet

  mailhog:
    image: mailhog/mailhog
    restart: always
    ports:
      - "8025:8025"
      - "1025:1025"
    networks:
      - cloudnet

volumes:
  db_data:
  nextcloud_data:

networks:
  cloudnet:
```

🚀 5. Container starten

sudo docker-compose up -d

Prüfen mit:

sudo docker ps

→ alle 4 Container sollten laufen

🔗 6. Dienste im Browser öffnen

Nextcloud: http://<ec2-ip>:8080

pgAdmin: http://<ec2-ip>:5050

Mailhog: http://<ec2-ip>:8025

📦 7. Nextcloud Setup abschließen

Admin-Benutzer anlegen

Datenbankauswahl:

Benutzer: nc_user

Passwort: nc_pass

Datenbankname: nextcloud

Datenbank-Host: db

Setup abschliessen → Nextcloud ist einsatzbereit

🛠️ 8. pgAdmin mit PostgreSQL verbinden

pgAdmin öffnen → Login mit admin@admin.com / admin123

"Neuer Server" → Name: Nextcloud-DB

Verbindung:

Hostname: db

Port: 5432

Benutzer: nc_user

Passwort: nc_pass

Datenbank: nextcloud

→ Du siehst alle Nextcloud-Tabellen (z. B. oc_filecache, oc_users)

✉️ 9. Mailhog als SMTP in Nextcloud einrichten

In Nextcloud: Admin → Einstellungen → Grundeinstellungen → E-Mail

Felder ausfüllen:

Mail-Modus: SMTP

Verschlüsselung: keine

Von-Adresse: admin@cloud.local

SMTP-Adresse: mailhog

Port: 1025

Kein Haken bei Authentifizierung!

Testmail senden an test@cloud.local

Öffne http://<ec2-ip>:8025 → Mail erscheint sofort

🧪 10. Alternativer Mail-Test mit mhsendmail (CLI)

wget https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64
chmod +x mhsendmail_linux_amd64
sudo mv mhsendmail_linux_amd64 /usr/local/bin/mhsendmail

# Test senden:
echo -e "Subject: Test\n\nDies ist ein Test." | mhsendmail --smtp-addr=mailhog:1025 test@cloud.local

→ Mail erscheint in Mailhog

✅ Gesamtfazit

Alle Komponenten wurden erfolgreich installiert, gestartet und integriert. Alle Dienste laufen gemeinsam in einem Docker-Netzwerk und können miteinander kommunizieren:

Nextcloud nutzt PostgreSQL als Datenbank

pgAdmin greift direkt auf dieselbe Datenbank zu

Mail-Versand erfolgt über Mailhog

Diese Umgebung eignet sich ideal zum Experimentieren, Testen und Präsentieren einer containerisierten Cloud-Infrastruktur.
