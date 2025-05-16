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
docker network inspect mil-mynetwork
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_18.png?raw=true" width="800" />

### Nicht verwendete Netzwerke entfernen
Um ein nicht mehr benötigtes Docker-Netzwerk zu entfernen, wurde der folgende Befehl verwendet:

```bash
docker network rm xxx-mynetwork
```

Dieser Befehl entfernt das Netzwerk mil-mynetwork, wenn es nicht mehr verwendet wird. Vorher kann überprüft werden, ob das Netzwerk noch verwendet wird, indem man es mit dem Befehl *docker network ls* auflistet.

## Quellen

Gitlab von Herr Rohr 
