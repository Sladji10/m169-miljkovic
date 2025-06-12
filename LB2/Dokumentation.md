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
