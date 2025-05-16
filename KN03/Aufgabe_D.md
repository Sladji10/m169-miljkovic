## Erstellung und Verwaltung von OCI-Images

### Ziel der Übung

Ziel dieser Übung war es, die grundlegenden Konzepte von Docker im Zusammenhang mit der Erstellung, Anpassung und Verwaltung von OCI-Images zu verstehen. Am Ende sollten folgende Bedingungen erfüllt sein:

### Ubuntu-Container starten und anpassen

Zunächst wurde ein Ubuntu-Container gestartet und mit zusätzlichen Softwarepaketen ausgestattet. Dies erfolgte über den folgenden Befehl:

```bash
docker run -it --name cowsay --hostname cowsay ubuntu bash
```
Dieser Befehl startet den Ubuntu-Container im interaktiven Modus und öffnet eine Bash-Shell, in der du Änderungen vornehmen kannst.

### Änderungen im Container vornehmen
Sobald der Container läuft, können Änderungen vorgenommen werden:

```bash
apt-get update
apt-get install -y cowsay fortune
```

### Container stoppen und starten
Nachdem die Änderungen im Container vorgenommen wurden, wurde der Container gestoppt und anschliessend mit den folgenden Befehlen neu gestartet:

```bash
docker start cowsay
```

Der Container wird nun wieder im Hintergrund ausgeführt.

### Änderungen im Container committen
Der Zustand des Containers wurde in ein neues Docker-Image übertragen, sodass die Änderungen erhalten bleiben:

```bash
docker commit cowsay img/cowsay
```

Dieser Befehl erstellt ein neues Image mit dem Namen *img/cowsay*, das die Änderungen aus dem Container enthält.

### Dockerfile erstellen und Image bauen
Ein Dockerfile wurde erstellt, um die Installation von cowsay und fortune zu automatisieren. Der folgende Befehl wurde verwendet, um das Docker-Image mit dem Dockerfile zu bauen:

```bash
docker build -t df/cowsay .
```

Das *Dockerfile* ist im selben Verzeichnis wie die *index.html* und enthält Anweisungen zum Installieren von cowsay und fortune.

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_17.png?raw=true" width="800" />

### Container aus dem neuen Image starten
Nach dem Bau des Docker-Images wurde ein neuer Container im Hintergrund gestartet, um den Webserver zugänglich zu machen:

```bash
docker run --rm -d --name cowsay -p 80:80 df/cowsay
```

*--rm* sorgt dafür, dass der Container nach dem Stoppen automatisch entfernt wird.

-p 80:80 leitet Port 80 des Containers auf Port 80 des Hosts weiter, sodass der Webserver über **http://Public-IP zugänglich ist.

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_16.png?raw=true" width="800" />

### Überprüfung und Tests
Der Webserver konnte erfolgreich über http://Public-IP erreicht werden, und das index.html wurde korrekt angezeigt.

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_15.png?raw=true" width="800" />

## Quellen

*Gitlab von Herr Rohr und das Tutorial Video*
