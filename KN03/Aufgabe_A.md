## Aufsetzen der EC2-Instanz mit IaC-Code - AWS LEARNER LAB

Ich habe erfolgreich eine Verbindung zu meiner AWS EC2-Instanz hergestellt. Der benötigte private SSH-Key (Sladjan.pem Datei) befindet sich lokal unter folgendem Pfad:

```
C:/Users/sladjan.miljkovic/m169/Sladjan.pem
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_4.png?raw=true" width="800" />

Mit diesem Befehl konnte ich mich per CMD auf meine erstelle EC2-Instanz verbinden: 

```
ssh -i "sladjan.pem" ubuntu@ec2-54-87-92-215.compute-1.amazonaws.com
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_6.png?raw=true" width="800" />

### Instanz-Konfiguration

Für die Umsetzung der Container-Challenges wurde eine neue EC2-Instanz in der AWS Management Console erstellt mit folgenden Einstellungen:

- **Instanzname:** `M169-Lab`
- **Image (AMI):** Ubuntu Server (neueste Version)
- **Instanztyp:** `t2.micro`
- **Key Pair:** neues Schlüsselpaar erstellt (für SSH-Zugriff)
- **Netzwerk:** Default VPC
- **Security Group:** `M169_Container` mit folgenden **Inbound-Regeln**:

| Type         | Protocol | Port | Source     | Zweck                            |
|--------------|----------|------|------------|----------------------------------|
| SSH          | TCP      | 22   | 0.0.0.0/0  | SSH-Zugriff                      |
| HTTP         | TCP      | 80   | 0.0.0.0/0  | Web-Zugriff                      |
| Custom TCP   | TCP      | 5000 | 0.0.0.0/0  | Portfreigabe für Container       |
| Custom TCP   | TCP      | 5001 | 0.0.0.0/0  | Portfreigabe für Folgeübungen    |
| All ICMP     | ICMP     | All  | 0.0.0.0/0  | Ping erlaubt (z. B. für Testing) |

## Cloud-Init Script

Im Abschnitt **User data** wurde folgendes Cloud-Init-Script verwendet, das beim Start automatisch ausgeführt wird:

```yaml
#cloud-config

packages:
  - apt-transport-https
  - ca-certificates
  - curl
  - gnupg-agent
  - software-properties-common

write_files:
  - path: /etc/sysctl.d/enabled_ipv4_forwarding.conf
    content: |
      net.ipv4.conf.all.forwarding=1

groups:
  - docker

system_info:
  default_user:
    groups: [docker]

runcmd:
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
  - add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  - apt-get update -y
  - apt-get install -y docker-ce docker-ce-cli containerd.io
  - systemctl start docker
  - systemctl enable docker
  - apt-get install podman -y
  - systemctl start podman
  - systemctl enable podman
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_5.png?raw=true" width="800" />

### Unterschied zwischen Docker und Podman

| Merkmal            | Docker                                 | Podman                                  |
|--------------------|----------------------------------------|------------------------------------------|
| **Architektur**     | Client-Server-Modell (Docker Daemon)   | Daemonlos    |
| **Rootless-Mode**   | Teilweise unterstützt                  | Standardmässig möglich                    |
| **Kompatibilität**  | Dockerfile, Docker CLI vollständig     | Kompatibel mit Docker                    |
| **Systemd-Integration** | Eingeschränkt                      | Sehr gute Unterstützung                  |
| **Sicherheit**      | Weniger isoliert (Daemon läuft als root) | Besser isoliert |

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_6.png?raw=true" width="800" />

### Warum ist podman.service inaktiv?

Podman ist **daemonlos** – das bedeutet, es benötigt keinen ständig laufenden Hintergrunddienst wie Docker. Stattdessen wird Podman direkt vom Benutzer aufgerufen und **startet Prozesse nur bei Bedarf**. Daher zeigt der Befehl `systemctl status podman` oft den Status `inactive (dead)` an, obwohl Podman **voll funktionsfähig** ist.

## Quellen

*Gitlab von Herr Rohr und ChatGPT für den Unterschied zwischen Docker und Podman + Warum podman.service inaktiv ist*
