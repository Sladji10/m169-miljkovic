## Docker Swarm Cluster aufsetzen - SETUP HIGH AVAILABILITY PLATFORM

### Zielsetzung

Ziel dieser Challenge war es, eine produktionsreife, hochverfügbare Plattform mit Docker Swarm aufzusetzen. Die Plattform besteht aus einem Cluster mit **5 Nodes** auf **AWS EC2-Instanzen**, verteilt über **zwei Availability Zones (AZs)**.

###  Setup-Details

| Komponente  | Anzahl | Beschreibung |
|-------------|--------|--------------|
| *Manager Nodes* | *3* | *Docker-Swarm-Manager, davon einer `Leader`, zwei `Reachable`* |
| *Worker Nodes*  | *2* | *Ausführung der Container* |
| *Availability Zones* | *2** | *z.B. `us-east-1a`, `us-east-1b`* |
| *Plattform* | *AWS EC2* | *Ubuntu 22.04 LTS, `t2.micro`* |
| *Netzwerke* | *Public + Private* | *Kommunikation über `172.31.0.0/16` CIDR* |
| *Cluster Kommunikation* | *Security Group* | *Inbound-Rules für Ports **22**, **2377**, **7946** (TCP/UDP), **4789** (UDP)* |

### EC2-Instanzen (5x)

- **3 Manager-Nodes**
- **2 Worker-Nodes**
- Verteilt über 2 Availability Zones
- `cloud-init` Script zur automatisierten Docker-Installation:

```bash
#!/bin/bash
apt-get update -y
apt-get install docker.io -y
systemctl enable docker
systemctl start docker
```

### Security Group Inbound-Rules

| Port | Protokoll | Zweck |
|------|-----------|-------|
| 22   | TCP       | SSH-Zugriff |
| 2377 | TCP       | Swarm Manager Kommunikation |
| 7946 | TCP/UDP   | Node-Kommunikation |
| 4789 | UDP       | Overlay Netzwerk |

### Swarm Initialisierung
Auf `manager-1`:
```bash
sudo docker swarm init --advertise-addr 172.31.85.233
```

### Andere Nodes hinzufügen
Auf allen anderen Nodes:
```bash
sudo docker swarm join --token SWMTKN-1-2o4v9p5d4cqs6ml7uyhzxgcxjfy0j88spystvlly01aoceoyzj-6h6nxgllffeuih2kqnmujy66n 172.31.85.233:2377
sudo docker swarm join --token SWMTKN-1-2o4v9p5d4cqs6ml7uyhzxgcxjfy0j88spystvlly01aoceoyzj-6h6nxgllffeuih2kqnmujy66n 172.31.85.233:2377
```

### Manager auf Drain setzen:
```bash
sudo docker node update --availability drain q7enflj9rutzb3et4gu1dlj2i
sudo docker node update --availability drain upd4pfzwuko96tbw9ctdq8t4b
sudo docker node update --availability drain n1lzb3op34szbsakzfmytcps6
```

Nur Worker-Nodes sollen Container ausführen dürfen.

### Nachweis

### ✅ 5 Nodes sichtbar im Cluster:
```bash
sudo docker node ls
```

screeen

### ✅ Manager und Worker-Nodes verteilt über 2 AZs:

Manager 1 und Worker 1 haben AZ: 

Manager 2, Manager 3 und Worker 2 haben AZ: 

Screenshot

---

### Fragen

### 🔹 **Was macht der Raft-Algorithmus?**
- Konsensmechanismus zur Manager-Wahl
- Verhindert Inkonsistenzen
- Es braucht ein **Quorum** für Entscheidungen (Mehrheit der Manager)

### 🔹 **Leader & Reachable**
- `Leader`: aktueller Hauptmanager, koordiniert den Cluster
- `Reachable`: bereit, Leader zu übernehmen
- Fällt der Leader aus → automatischer Leader-Wechsel

### 🔹 **Drain vs. Active**
- `Drain`: Node erhält keine neuen Container, vorhandene werden migriert
- `Active`: Node darf Container ausführen

## Quellen


