## Docker Swarm Imperativ - CONTAINER ORCHESTRATION: ENTRY-LEVEL

### Ziel der Übung

- Bereitstellung eines Webservices mit Docker Swarm (imperative Methode)
- Skalierung von Services (Replicas)
- Load Balancing & Self-Healing überprüfen
- Arbeiten mit einem 5-Node-Cluster (3 Manager, 2 Worker)

### Cluster-Struktur (EC2 auf AWS)

| Rolle           | Anzahl | Private-IPs (intern)        |
|------------------|--------|------------------------------|
| Manager-Nodes    | 3      | `ip-172-31-85-233`, `ip-172-31-69-205`, `ip-172-31-73-187` |
| Worker-Nodes     | 2      | `ip-172-31-81-153`, `ip-172-31-65-25` |

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_39.png?raw=true" width="850" />

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_40.png?raw=true" width="850" />

**(2 verschiedene AZ)**

### Service erstellen (auf einem **Manager-Node**)

```bash
sudo docker service create \
--name 169-web \
--publish published=5010,target=8080 \
--replicas 5 \
marcellocalisto/webapp_one:1.0
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_33.png?raw=true" width="850" />

### Service-Status prüfen (Manager-Node)

```bash
sudo docker service ls
sudo docker service ps 169-web
```

**✅ Ergebnis:**

- 5 Container laufen (Verteilt auf die beiden Worker-Nodes)

### Web-App im Browser aufrufen

In der AWS Console, die die Public IP von Worker 4 kopieren und im Browser mit :5010 öffnen.

*Beispiel (meine Public-IP):*

```bash
http://34.207.161.119:5010 
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_36.png?raw=true" width="850" />

**✅ Seite zeigt:**

"Diese Web-App läuft in einem Container"

Container-ID ändert sich bei jedem Reload = Load Balancing

## Service auf 10 Container skalieren

```bash
sudo docker service scale 169-web=10
```

```bash
sudo docker service ps 169-web
```

*Container gleichmässig verteilt auf die zwei Worker-Nodes (5 + 5)*

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_33.png?raw=true" width="850" />

### 3 Container auf Worker 2 löschen

**SSH auf Worker 2:**

```bash
ssh -i "C:\Users\sladjan.miljkovic\m169\sladjan.pem" ubuntu@ec2-3-239-82-15.compute-1.amazonaws.com
```

```bash
docker container ls
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_34.png?raw=true" width="850" />

**3 Container löschen:**

```bash
docker container rm -f <ID1> <ID2> <ID3>
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_37.png?raw=true" width="850" />

### Self-Healing beobachten (Manager-Node)

```bash
sudo docker service ps 169-web
```

*Docker Swarm erkennt, dass 3 Container fehlen → sie werden automatisch neu gestartet.*

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_38.png?raw=true" width="850" />

### Begriff	Bedeutung

- **Desired State:**	Zielzustand (z. B. 10 Container sollen laufen)
- **Observed State:**	Tatsächlich laufende Container
- **Self-Healing:**	Swarm stellt Container automatisch wieder her, wenn sie gelöscht wurden
- **Load Balancing:**	Traffic wird gleichmässig auf Container verteilt

### Clean-up

```bash
sudo docker service rm 169-web
sudo docker service ls
```

## Quellen

Gitlab Repository von Herr Rohr
