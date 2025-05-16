## OCI-Images mit Docker - RUN & ADMINISTRATION

### Ziel der Übung
Das Ziel dieser Übung war es, die grundlegenden Konzepte von Docker im Zusammenhang mit MariaDB zu verstehen, insbesondere wie man eine Datenbank in einem Container betreibt und Daten sicher speichert, auch wenn der Container neu gestartet oder entfernt wird. Am Ende sollte eine funktionierende MariaDB-Umgebung in Docker existieren, die mit HeidiSQL verwaltet wird, und die Datenbank sollte durch Docker Volumes persistent gespeichert werden.

### **HeidiSQL-Client herunterladen und installieren**
Zuerst wurde der HeidiSQL-Client heruntergeladen und installiert, um eine Verbindung zur MariaDB-Datenbank im Docker-Container herzustellen.

- **Download-Link**: [HeidiSQL Download](https://www.heidisql.com/download.php)

### **Überprüfung der Docker-Installation**
Die Docker-Installation wurde durch den folgenden Befehl überprüft:
```bash
docker --version
```

### MariaDB Docker-Image herunterladen und Container starten
Das MariaDB Docker-Image wurde mit folgendem Befehl heruntergeladen:

```bash
docker pull mariadb
```

Dann wurde der MariaDB-Container mit dem folgenden Befehl gestartet:

```bash
docker run -d --name mariadb-container -e MYSQL_ROOT_PASSWORD=rootpassword -p 3306:3306 mariadb
```

### Port-Forwarding aktivieren und EC2-Security Group konfigurieren
Um auf den MariaDB-Container zuzugreifen, wurde das Port-Forwarding im Docker-Container eingerichtet. Ausserdem wurde die EC2-Security Group angepasst, um den Zugriff auf Port 3306 zu erlauben. Hierfür wurde in der AWS Management Console eine Inbound-Regel hinzugefügt:

- **Typ:** Custom TCP Rule
- **Port:** 3306
- **Quelle:** 0.0.0.0/0

### Verbindung mit HeidiSQL herstellen
In HeidiSQL wurde eine neue Sitzung eingerichtet:

- **Hostname/IP:** Die öffentliche IP-Adresse der EC2-Instanz *(98.81.124.170)*

- **Benutzername:** root

- **Passwort:** rootpassword

- **Port:** 3306

### Datenbank erstellen
Eine neue Datenbank: M169_KN03_MIL wurde in HeidiSQL erstellt.

### Docker Volume einrichten
Ein Docker Volume wurde erstellt, um die Datenbankdaten dauerhaft zu speichern, auch nach einem Neustart des Containers:

```bash
docker volume create mariadb-data
```

Der Container wurde mit dem Volume wie folgt gestartet:

```bash
docker run -d --name mariadb-container -e MYSQL_ROOT_PASSWORD=rootpassword -v mariadb-data:/var/lib/mysql -p 3306:3306 mariadb
```

### Testen der Persistenz
Um die Persistenz zu testen, wurde der Container gelöscht und anschliessend neu gestartet:

```bash
docker rm -f mariadb-container
docker run -d --name mariadb-container -e MYSQL_ROOT_PASSWORD=rootpassword -v mariadb-data:/var/lib/mysql -p 3306:3306 mariadb
```

Nach dem Neustart des Containers konnte die zuvor erstellte Datenbank weiterhin über HeidiSQL aufgerufen werden.
