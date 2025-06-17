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

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_9.png?raw=true" width="900" />

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_10.png?raw=true" width="900" />

## 🟢 2. Docker & Docker Compose installieren

```yaml
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
```

## 🟢 3. Projektverzeichnis anlegen

```yaml
mkdir LB2
cd LB2
```

## 🟢 4. docker-compose.yml erstellen (Im LB2 Ordner)

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

## 🟢 5. Container starten

```yaml
sudo docker-compose up -d
```

Prüfen mit:

```yaml
sudo docker ps
```

*→ alle 4 Container sollten laufen*

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_11.png?raw=true" width="900" />

## 🟢 6. Dienste im Browser öffnen

- **Nextcloud:** http://18.205.156.78:8080 (Öffentliche IP Stand: **16.06.2025**)

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_5.png?raw=true" width="750" />

- **pgAdmin:** http://18.205.156.78:5050 (Öffentliche IP Stand: **16.06.2025**)

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_4.png?raw=true" width="750" />

- **Mailhog:** http://18.205.156.78:8025 (Öffentliche IP Stand: **16.06.2025**)

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_12.png?raw=true" width="750" />


## 🟢 7. Nextcloud Setup abschliessen

- Admin-Benutzer anlegen
- Datenbankauswahl:
  - Benutzer: **nc_user**
  - Passwort: **nc_pass**
  - Datenbankname: **nextcloud**
  - Datenbank-Host: **db**
- Setup abschliessen → **Nextcloud** ist einsatzbereit

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_5.png?raw=true" width="750" />

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_13.png?raw=true" width="750" />

## 🟢 8. pgAdmin mit PostgreSQL verbinden

- pgAdmin öffnen → Login mit **admin@admin.com** / **admin123**
- "Neuer Server" → Name: **Nextcloud-DB**
- Verbindung:
  - Hostname: **db**
  - Port: **5432**
  - Benutzer: **nc_user**
  - Passwort: **nc_pass**
  - Datenbank: **nextcloud**
- → Du siehst alle Nextcloud-Tabellen *(z.B. oc_filecache, oc_users)*

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_3.png?raw=true" width="750" />

## 🟢 9. Mailhog als SMTP in Nextcloud einrichten

- In Nextcloud: ***Admin → Einstellungen → Grundeinstellungen → E-Mail***

  - **Mail-Modus:** SMTP
  - **Verschlüsselung:** keine
  - **Von-Adresse:** admin@cloud.local
  - **SMTP-Adresse:** mailhog
  - **Port:** 1025
  - Kein Haken bei Authentifizierung!

- Testmail senden an **test@cloud.local**
- ```http://18.205.156.78:8025``` → *Mail erscheint sofort*

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_8.png?raw=true" width="750" />

## 🟢 10. Alternativer Mail-Test mit mhsendmail

```yaml
wget https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64
chmod +x mhsendmail_linux_amd64
sudo mv mhsendmail_linux_amd64 /usr/local/bin/mhsendmail
```

**Test senden:**
```yaml
echo -e "Subject: Test\n\nDies ist ein Test." | mhsendmail --smtp-addr=mailhog:1025 test@cloud.local
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_6.png?raw=true" width="750" />

- **→ Mail erscheint in Mailhog**

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/2_7.png?raw=true" width="750" />

## ✅ 11. Gesamtfazit

Alle Komponenten wurden erfolgreich installiert, gestartet und integriert. Alle Dienste laufen gemeinsam in einem Docker-Netzwerk und können miteinander kommunizieren:

- **Nextcloud nutzt PostgreSQL als Datenbank**
- **pgAdmin greift direkt auf dieselbe Datenbank zu**
- **Mail-Versand erfolgt über Mailhog**

Diese Umgebung eignet sich ideal zum Experimentieren, Testen und Präsentieren einer containerisierten Cloud-Infrastruktur.

## Quellen

Gitlab Repository von *Herr Rohr*, **ChatGPT Erklärungen:** *Nextcloud, Mailhog*, **YouTube Video:** *Mailhog Mail Versand*
