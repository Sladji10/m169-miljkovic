## OCI-Images, Container und Registry - BASICS 

### Verbindung via SSH zur AWS EC2-Instanz

- Verbinden mit meiner EC2-Instanz mit folgendem Befehl:

```bash
ssh -i "sladjan.pem" ubuntu@ec2-54-87-92-215.compute-1.amazonaws.com
```

Ich habe einen Webserver gestartet in einem Container, der NGINX verwendet.

```bash
docker run -d -p 8080:80 nginx
```

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_9.png?raw=true" width="800" />

### Fehlerbehebung: Webseite nicht erreichbar?

Falls die Webseite nicht geladen wird, liegt das oft an den Sicherheitsregeln der EC2-Instanz (Security Groups). So kannst du das Problem beheben:

1. Gehe in AWS Management Console zu **EC2 > Security Groups**.

2. Wähle die Security Group aus, die bei der EC2-Instanz zugeordnet ist.

3. Ergänze eine neue **Inbound-Regel** mit folgenden Einstellungen:

| Typ         | Protokoll | Portbereich | Quelle     |
|-------------|-----------|-------------|------------|
| Custom TCP  | TCP       | 8080        | 0.0.0.0/0  |

4. Speichere die Regel.

5. Versuche erneut, die Webseite im Browser über `http://54.87.92.215:8080` (Meine Public IP) aufzurufen.



<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_7.png?raw=true" width="800" />

<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_8.png?raw=true" width="800" />


<img src="https://github.com/Sladji10/m169-miljkovic/blob/main/Screenshots/1_10.png?raw=true" width="800" />
