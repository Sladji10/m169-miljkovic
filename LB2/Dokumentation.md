# Private Cloud mit Nextcloud, pgAdmin und Mailhog auf AWS EC2 mit Docker

## Projektübersicht

**Ziel ist es, eine vollständige private Cloud-Umgebung auf einer AWS EC2-Instanz zu betreiben, in der folgende Dienste containerisiert laufen:**

- **Nextcloud** (Cloud-Speicherlösung)
- **PostgreSQL** (Datenbank)
- **pgAdmin 4** (Web-Interface zur Verwaltung von PostgreSQL)
- **Mailhog** (lokaler SMTP-Mailserver zum Testen)

Alle Dienste sollen in einem Docker-Netzwerk verbunden werden. Der Zugang erfolgt über das Webinterface, Konfiguration über Docker Compose. Alle Schritte werden einzeln dokumentiert.

## Systemumgebung mit Netzwerkplan

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/netz.png?raw=true" width="750" />



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

## 🟢 2. Docker und Docker Compose installieren

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

## 🟢 11. Security-Aspekte

Die Sicherheit der Umgebung wurde durch mehrere Massnahmen verbessert und dokumentiert:

## Begrenzung der Zugriffe (AWS Security Group)

Es wurden **nur notwendige Ports geöffnet**:

- `22` für SSH (nur eigene IP erlaubt)
- `8080` für Nextcloud
- `5050` für pgAdmin
- `8025` und `1025` für Mailhog

➡️ **Alle anderen Ports sind gesperrt.** Dadurch wird das System von unnötigem externen Zugriff geschützt.

### Isolierte Container mit internem Docker-Netzwerk

Alle Dienste sind über ein eigenes Docker-Netzwerk (`cloudnet`) verbunden.

- Die **PostgreSQL-Datenbank ist nicht direkt von aussen erreichbar**
- Nur **Nextcloud** und **pgAdmin** können intern darauf zugreifen

➡️ Dies schützt die Datenbank vor Angriffen von aussen.

### Sichere Zugangsdaten

- Für PostgreSQL und Nextcloud wurden **eigene Benutzer und Passwörter** gesetzt.
- Das **Admin-Passwort für Nextcloud** ist stark und sicher gewählt.
- **pgAdmin-Zugangsdaten** sind individuell gesetzt und nicht öffentlich sichtbar.

### Hinweis zu Mailhog

Mailhog wurde **ohne Authentifizierung** eingerichtet, was **nur für Testumgebungen** geeignet ist.

➡️ In einer **Produktivumgebung** sollte Mail verschlüsselt und mit Authentifizierung versendet werden.


## 🟢 12. Systemüberwachung (Monitoring)

In meinem Projekt überwache ich die laufenden Container mit folgenden Tools:

### Echtzeit-Überwachung mit `sudo docker stats`

Mit dem Befehl `sudo docker stats` kann ich die CPU- und RAM-Auslastung der laufenden Container prüfen. Beispiel aus meinem Projekt:

```bash
sudo docker stats
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/p1.png?raw=true" width="750" />

**Die Werte zeigen:**

  - **lb2_nextcloud_1** verwendet aktuell ca. *71 MiB RAM* und keine *CPU*

  - **lb2_pgadmin_1** hat den höchsten RAM-Verbrauch mit ca. *236 MiB* bei sehr geringer CPU-Last

  - **lb2_mailhog_1** und **lb2_db_1** zeigen ebenfalls unkritische Werte

**Fazit**: Alle Container laufen stabil, ohne nennenswerte Auslastung.

### Logüberwachung mit `sudo docker-compose logs`

Um mögliche Fehler zu erkennen, schaue ich regelmässig in die Logs des Containers:

```bash
sudo docker-compose logs -f nextcloud
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/p2.png?raw=true" width="750" />

Die Logs zeigen, dass der Nextcloud-Container korrekt gestartet ist.

### Statusprüfung aller Container mit `sudo docker ps`

Zur Überprüfung, ob alle Container aktiv und fehlerfrei laufen, verwende ich:

```bash
sudo docker ps
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/p3.png?raw=true" width="750" />

Alle Container laufen seit über einer Stunde ohne Probleme. Es gibt keine Container im Status Exited oder Restarting.

### Fazit

Die Systemüberwachung in meinem Projekt erfolgt mit folgenden einfachen, aber effektiven Befehlen:

  - `sudo docker stats`: Live-Monitoring von Ressourcenverbrauch (CPU, RAM)

  - `sudo docker-compose logs`: Überwachung der Container-Logs auf Fehler oder Hinweise

  - `sudo docker ps`: Übersicht über den Zustand und die Laufzeit aller Container

Diese Kombination reicht für mein Projekt aus, um die Services sicher und effizient zu überwachen. Für grössere Setups wären Tools wie ***Prometheus***, ***Grafana*** oder ***cAdvisor*** empfehlenswert, um historische Daten visuell auszuwerten. 

## ✅ 13. Gesamtfazit

Alle Komponenten wurden erfolgreich installiert, gestartet und integriert. Alle Dienste laufen gemeinsam in einem Docker-Netzwerk und können miteinander kommunizieren:

- **Nextcloud nutzt PostgreSQL als Datenbank**
- **pgAdmin greift direkt auf dieselbe Datenbank zu**
- **Mail-Versand erfolgt über Mailhog**

Diese Umgebung eignet sich ideal zum Experimentieren, Testen und Präsentieren einer containerisierten Cloud-Infrastruktur.

## Quellen

Gitlab Repository von *Herr Rohr*, **ChatGPT Erklärungen:** *Nextcloud, Mailhog*, **YouTube Video:** *Mailhog Mail Versand*
