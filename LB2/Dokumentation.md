# Private Cloud mit Nextcloud auf AWS EC2 mit Docker

**Nextcloud Docker:**
- Nextcloud ist eine App, mit der du Dateien speichern und verwalten kannst, ähnlich wie Google Drive. Sie eignet sich gut für Container, weil sie einfach zu handhaben und leicht ist. Du kannst deine Dateien in einer privaten Cloud speichern und hast die Kontrolle darüber.

**Docker-Compose:**

- Docker-Compose wurde verwendet, um mehrere Container zu orchestrieren (Nextcloud und PostgreSQL-Datenbank). Mit Docker-Compose kannst du die Konfiguration und Verwaltung von Containern vereinfachen.

**PostgreSQL Docker:**
- Ich habe PostgreSQL als Datenbank gewählt, um Nextcloud zu unterstützen, da es eine der von Nextcloud empfohlenen Datenbanken ist.

## Projektstruktur und Scrum-Prozess

### Sprint 1: Grundfunktionalität

**Ziele:**

- Einen lauffähigen Teil der Anwendung entwickeln.
- Eine Möglichkeit zur Datenspeicherung integrieren.
- Anwendung so vorbereiten, dass sie in Containern läuft.

**Meilensteine:**

***1. Projekt starten (Repository anlegen, Grundstruktur erstellen):***

  - Erstelle ein GitLab-Repository und lege die grundlegende Ordnerstruktur an.

    - ```/frontend``` für das React-Frontend.

    - ```/backend``` für die Node.js API.

    - ```/database``` für PostgreSQL.

    - ```/cache``` für Redis.

  - Füge eine README.md und eine docker-compose.yml-Datei hinzu.

***2. Erste Funktionen umsetzen (z.B. speichern, anzeigen, verarbeiten):***

- Backend: Erstelle ein einfaches API, das To-Do-Elemente speichert, anzeigt und löscht.

- Frontend: Baue eine einfache React-Anwendung, um die To-Do-Liste darzustellen.

***3. Anwendung in Containern ausführen und testen:***

- Erstelle Docker-Images für das Backend und das Frontend.

- Konfiguriere die Datenbank und den Cache als Container.

- Teste das Setup lokal.
