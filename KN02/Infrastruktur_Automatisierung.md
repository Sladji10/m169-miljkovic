## Cloud-Service-Modelle
*Hier ist eine Übersicht über die vier Modelle:*

<img src="https://github.com/Sladji10/m346-miljkovic/blob/main/Images/XaaS.png?raw=true" width="400" />

### Infrastructure as a Service (IaaS)
***Einfache Erklärung:*** Du bekommst die grundlegende IT-Infrastruktur aus der Cloud. Das bedeutet, du mietest virtuelle Maschinen, Speicherplatz und Netzwerke. Wenn du zum Beispiel einen eigenen Server betreiben willst, aber nicht die Hardware kaufen und selbst verwalten möchtest. Stell dir vor, du mietest einen Computer im Internet, auf dem du alles installieren kannst, was du möchtest.

***Anbieter:*** Amazon Web Services, Digital Ocean, Microsoft Azure, Google Cloud.

### Platform as a Service (PaaS)
***Einfache Erklärung:*** Du bekommst eine Plattform aus der Cloud, auf der du Anwendungen entwickeln und betreiben kannst. Die Infrastruktur darunter (Server, Speicher usw.) wird für dich verwaltet. Wenn du dich nur auf die Entwicklung deiner Software konzentrieren willst, ohne dich um die Server kümmern zu müssen. Stell dir vor, du hast eine Werkbank mit allen Werkzeugen, die du brauchst, um etwas zu bauen. Aber du musst dich nicht darum kümmern, wo die Werkbank steht oder wie die Werkzeuge gewartet werden.

***Anbieter:*** Heroku, Amazon mit AWS Elastic Beanstalk, Digital Ocean, Microsoft Azure, Google Cloud.

### Software as a Service (SaaS)
***Einfache Erklärung:*** Du nutzt eine fertige Software, die in der Cloud läuft, ohne sie auf deinem Computer installieren zu müssen. Wenn du eine Software sofort und ohne Installation nutzen möchtest. Stell dir vor, du gehst in eine Bibliothek und liest ein Buch, ohne es kaufen zu müssen. Du nutzt einfach eine Software wie Google Docs oder Gmail direkt im Browser.

***Anbieter:*** Vendor-Lock-In

### Function as a Service (FaaS)
***Einfache Erklärung:*** Du schreibst kleine Programmfunktionen, die in der Cloud ausgeführt werden, und zahlst nur für die Ausführung dieser Funktionen. Wenn du nur kleine Aufgaben erledigen möchtest, ohne dich um Server oder Infrastruktur zu kümmern. Stell dir vor, du schreibst eine kurze Notiz, die jemand für dich aufbewahrt und immer dann vorliest, wenn es nötig ist. Du bezahlst nur, wenn die Notiz vorgelesen wird.

***Anbieter:*** Vendor-Lock-In

### Container as a Service (CaaS) 

***Einfache Erklärung:*** Diese Ebene ist dafür zuständig, containerisierten Workload auf den Ressourcen auszuführen, die eine IaaS-Cloud zur Verfügung stellt. Die Technologien dieser Ebene wie Docker, Kubernetes oder Mesos sind allesamt quelloffen verfügbar. Somit kann man sich seine private Cloud ohne Gefahr eines Vendor Lock-ins aufbauen.

## 1. Teil-Challenge

### 1. Aufgabe

- **IaaS** (Infrastructure as a Service): Hosting von virtuellen Maschinen
- **SaaS** (Software as a Service): Nutzung von Google Docs für die Textbearbeitung
- **CaaS** (Container as a Service): Containerverwaltung mit Kubernetes

### 2. Aufgabe – Unterschied zwischen IaaS und PaaS

**IaaS:** Du bekommst die Grund-Infrastruktur wie Server und Speicher. Du musst selbst Betriebssystem, Software und Anwendungen installieren und verwalten.

**PaaS:** Die Plattform mit Betriebssystem und Entwicklungsumgebung wird vom Anbieter bereitgestellt. Du kümmerst dich nur um die Entwicklung und Verwaltung deiner Anwendungen.

### Begriff Infrastructure As Code
Infrastructure As Code (IaC) bedeutet, dass du deine gesamte IT-Infrastruktur (wie Server, Netzwerke) mit Code definierst, anstatt alles manuell einzurichten. 

### Produkte, die Infrastructure As Code anbieten
*Terraform:* Ein Tool, um Infrastruktur in verschiedenen Cloud-Umgebungen mit Code zu verwalten.

*Ansible:* Automatisiert die Verwaltung und Konfiguration von Computern.

*Puppet:* Ermöglicht die Verwaltung der Konfiguration von Servern durch Code.

## 2. Teil-Challenge

### 1. Aufgabe: Automatisierung vs. Manuelle Konfiguration

**Warum ist es besser, IT-Systeme automatisch zu konfigurieren statt manuell?**

- Automatische Konfiguration ist schneller und weniger fehleranfällig.
- Sie sorgt für Konsistenz und Wiederholbarkeit der Systeme.
- Änderungen können dokumentiert und versioniert werden.
- Automatisierung spart Zeit und reduziert den Aufwand bei Updates oder Skalierungen.

### 2. Aufgabe: IaC-Tools & Container-Orchestrierung

**Zwei IaC-Tools:**

**Docker:**
Erstellt und verwaltet Container, in denen Anwendungen und ihre Umgebung isoliert laufen.
Vorteil: Anwendungen sind portabel und laufen überall gleich.

**Terraform:**
Ein Tool, um Infrastruktur (Cloud-Ressourcen, Netzwerke usw.) mit Code zu definieren und bereitzustellen.
Vorteil: Infrastruktur kann automatisiert und versioniert werden.

**Kubernetes und Container-Orchestrierung:**

- Kubernetes ist ein System zur Verwaltung von Container-Anwendungen.
- Es automatisiert das Starten, Skalieren und Überwachen von Containern.
- Container-Orchestrierung bedeutet, mehrere Container über viele Server verteilt zu verwalten.
- **Ziel:** Hohe Verfügbarkeit, Lastverteilung und einfache Verwaltung von Anwendungen.

### Amazon Web Services (AWS)

- Marktführer im Cloud-Computing mit 31 Regionen und 99 Verfügbarkeitszonen weltweit.
- Bietet virtuelle Server (EC2), Speicher (S3), serverloses Computing (Lambda), Datenbanken (RDS) und Content-Delivery (CloudFront).
- Stark in Ausfallsicherheit und globaler Verfügbarkeit.

### Microsoft Azure

- Bekannt für gute Integration mit Microsoft-Produkten und Hybrid-Cloud-Lösungen.
- Verfügt über 60+ Regionen mit mehreren Verfügbarkeitszonen für Ausfallsicherheit.
- Bietet virtuelle Maschinen, Speicher (Blob Storage), serverlose Funktionen, Cloud-Datenbanken und Identitätsmanagement (Active Directory).

### Google Cloud Platform (GCP)
- Fokus auf Innovation, KI und maschinelles Lernen.
- Hat 38 Regionen und über 100 Verfügbarkeitszonen mit schnellem Netzwerk.
- Bietet virtuelle Maschinen (Compute Engine), Speicher (Cloud Storage), Data-Warehouse (BigQuery), serverlose Funktionen und Kubernetes (GKE).

## 3. Teil Challenge

### 1. Aufgabe – Vergleich der globalen Infrastruktur

| Cloud-Plattform  | Anzahl Regionen | Anzahl Verfügbarkeitszonen (Availability Zones) |
|------------------|-----------------|-------------------------------------------------|
| AWS              | 31              | 99                                              |
| Microsoft Azure  | 60+             | Mehrere pro Region                               |
| Google Cloud     | 38              | Über 100                                        |

---

### 2. Aufgabe – Vergleich eines Dienstes (Compute) zwischen AWS, Azure und GCP

| Plattform         | Dienstname             | Beschreibung und Unterschiede                                      | Quelle                                           |
|-------------------|------------------------|-------------------------------------------------------------------|-------------------------------------------------|
| AWS               | Amazon EC2             | Skalierbare virtuelle Server mit vielen Instanztypen und hoher Flexibilität. Unterstützt Auto-Scaling und diverse Betriebssysteme. | [AWS EC2 Docs](https://docs.aws.amazon.com/ec2/) |
| Microsoft Azure   | Azure Virtual Machines | Bietet VMs für Windows und Linux mit einfacher Integration in Microsoft-Dienste. Starke Hybrid-Cloud-Unterstützung. | [Azure VMs Docs](https://learn.microsoft.com/azure/virtual-machines/) |
| Google Cloud      | Compute Engine         | Hoch skalierbare virtuelle Maschinen mit schneller Bereitstellung. Gute Performance dank Googles Netzwerk. | [GCP Compute Engine](https://cloud.google.com/compute) |



## Quellen
*GitLab von Herrn Rohr*
