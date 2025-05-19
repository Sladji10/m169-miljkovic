## Docker Compose - Container Orchestrierung mit mehreren Services - CONTAINER MANAGEMENT: ENTRY-LEVEL

### Übersicht
In diesem Projekt wurde eine Multicontainer-Webanwendung mit Docker Compose auf einer **AWS EC2 Instanz** bereitgestellt. Die Services werden per Compose-Datei orchestriert. Ziel ist es, eine einfache Seite zu erstellen, die beim Laden die Anzahl der Seitenaufrufe über Redis zählt.

### Voraussetzungen
- Docker & Docker Compose auf EC2 installiert
- Security Group mit freigegebenem Port 5169 (AWS)
- EC2-Öffentliche-IP: ```http://54.234.169.42:5169``` (Meine IP)

### Anpassungen **Dateien im geklonten Repo**

- **Published Port:** 5169
- **Target Port:** 8169
- **Volume:** mil-vol
- **Netzwerk:** mil-net
- **Flask-App Port in app.py:** 8169
- **Persönliches Logo** und **Text** angepasst in **index.html**

### Docker Compose starten
```docker compose down --volumes```

```docker compose up --build -d```

```docker compose ps```



### Webseite aufrufen im Browser:

```http://54.234.169.42:5169```

Bei mir sollte man ein Bild von Messi sehen.

### Volume und Netzwerk prüfen

### Umgebung löschen
- ```docker compose down --volumes```
- ```docker image ls```
- ```docker container ls -a```
- ```docker volume ls```
- ```docker network ls```

### Was ist Infrastructure as Code (IaC)?
Mit IaC beschreibt man Infrastruktur wie z. B. Container, Netzwerke oder Volumes in Code – z. B. in compose.yaml. Dadurch kann alles automatisiert und reproduzierbar erstellt werden.

**Beispiel:**

```yaml
services:
  web-fe:
    build: .
    ports:
      - target: 8169
        published: 5169
```

### Frontend & Backend

**Frontend:** Flask-Webseite mit HTML-Ausgabe

**Backend:** Redis-Datenbank für Aufrufzähler

**Vorteil:** Microservices sind unabhängig entwickelbar, leichter wartbar und skalierbar.


### Was ist ein Manifest?

Ein Manifest ist eine Datei, die die gewünschte Systemstruktur beschreibt.

**Beispiel:** ```compose.yaml```

**Dateiendungen:** ```.yaml oder .yml```

### Desired vs. Observed State

- **Desired State:** z.B. 2 Container sollen laufen (laut Compose-Datei)

- **Observed State:** z.B. 1 Container läuft tatsächlich

*Docker Compose gleicht automatisch beides ab.*


## Quellen

Gitlab von Herr Rohr und ChatGPT bei Fehlerbehebungen
