## Container Netzwerk - VERTIEFUNG

### Ziel

Ziel dieser Übung war es, die grundlegenden Konzepte von Docker-Netzwerken zu verstehen, insbesondere die Kommunikation zwischen Containern, die Erstellung und Verwaltung von Netzwerken sowie die verschiedenen Netzwerkmodi.

### Bestehende Docker-Netzwerke anzeigen

Um alle bestehenden Docker-Netzwerke anzuzeigen, wurde der folgende Befehl verwendet:

```bash
docker network ls
```

### Neues Netzwerk erstellen
Ein neues Docker-Netzwerk mit dem Namen mil-mynetwork wurde erstellt.

```bash
docker network create mil-mynetwork
```

Nach der Erstellung des Netzwerks konnte ich mit dem folgenden Befehl die Details des neuen Netzwerks anzeigen:

```bash
docker network inspect xxx-mynetwork
```



3. MySQL-Container im neuen Netzwerk starten
Ein MySQL-Container wurde im neu erstellten Netzwerk xxx-mynetwork gestartet. Der folgende Befehl wurde verwendet:

bash
Kopieren
docker run -d --name my-mysql --network xxx-mynetwork -e MYSQL_ROOT_PASSWORD=secret mysql
Dieser Befehl startet einen MySQL-Container im Hintergrund (-d), verbindet ihn mit dem Netzwerk xxx-mynetwork und setzt das Root-Passwort auf secret.

4. Überprüfung des Containers im Netzwerk
Um sicherzustellen, dass der MySQL-Container im richtigen Netzwerk läuft, wurde der folgende Befehl verwendet:
```bash
docker network inspect xxx-mynetwork
Die Ausgabe zeigt, dass der MySQL-Container im Netzwerk xxx-mynetwork läuft:


5. Nicht verwendete Netzwerke entfernen
Um ein nicht mehr benötigtes Docker-Netzwerk zu entfernen, wurde der folgende Befehl verwendet:

bash
Kopieren
docker network rm xxx-mynetwork
Dieser Befehl entfernt das Netzwerk xxx-mynetwork, wenn es nicht mehr verwendet wird. Vorher kann überprüft werden, ob das Netzwerk noch verwendet wird, indem man es mit dem Befehl docker network ls auflistet.

