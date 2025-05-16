
## Docker Image aufsetzen, in Registry ablegen und deployen - OCI: BASIC WORKFLOW

### Übersicht

In diesem Projekt wird ein Docker-Container auf einer AWS EC2-Instanz ausgeführt, der eine einfache Webanwendung hostet. Das Docker-Image wird aus einem GitHub-Repository gebaut und in der GitHub Container Registry gespeichert. Der Webserver läuft auf Port **8091**.

### Repository klonen

Klonen des GitHub-Repositories und ins Verzeichnis wechseln:

```bash
git clone https://github.com/ser-cal/container-bootstrap.git
cd container-bootstrap
```

### Docker-Image erstellen
Wechsle ins container-Verzeichnis und baue das Docker-Image:

```bash
cd container
docker image build -t ghcr.io/sladji10/m169-miljkovic/webserver_one:1.0 .
docker image ls | grep -i webserver
```

### Mit GitHub Container Registry einloggen
Um das Docker-Image zu GitHub Container Registry zu pushen, logge dich zuerst bei GitHub ein:

```bash
docker login ghcr.io
```

*(Gib deinen GitHub-Benutzernamen und ein Personal Access Token als Passwort ein)*

### Docker-Image auf GitHub pushen
Push das Docker-Image in die GitHub Container Registry:

```bash
docker push ghcr.io/dsladji10/m169-miljkovic/webserver_one:1.0
```

### Docker-Image auf EC2 starten
Starten des Docker-Containers auf deiner EC2-Instanz und Weiterleitung von Port 8091:

```bash
docker container run -d --name container-webserver -p 8091:8080 ghcr.io/sladji10/m169-miljkovic/webserver_one:1.0
```

### Sicherheitsgruppe anpassen
Stelle sicher, dass Port 8091 in der Security Group deiner EC2-Instanz freigegeben ist. Gehe dazu wie folgt vor:

.**Gehe zu EC2 > Security Groups in der AWS Management Console.**

Wähle die **Sicherheitsgruppe.** deiner EC2-Instanz aus.

Füge eine **neue Inbound-Regel** hinzu:

- **Typ:** HTTP
- **Protokoll:** TCP
- **Portbereich:** 8091
- **Quelle:** 0.0.0.0/0

### Überprüfen
Webbrowser öffnen:

*http://3.87.154.73:8091* (Das ist meine Public IP von der EC2 Instanz)

### Container stoppen und löschen
Wenn du den Container stoppen und das Image löschen möchtest, benutze folgende Befehle:

```bash
docker container stop container-webserver
docker container rm container-webserver
docker image rm ghcr.io/sladji10/m169-miljkovic/webserver_one:1.0
```

## Quellen

Gitlab von Herr Rohr und Tutorial Video + ChatGPT hat mir bei Problemen geholfen
