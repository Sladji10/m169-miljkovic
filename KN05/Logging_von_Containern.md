## Logging von Containern

### Start eines Containers mit Standard-Log-Driver und Logs abrufen

*Container starten:*

```bash
docker run --name m169_logtest ubuntu bash -c 'echo "Hallo"; echo "Ciao" >&2'
```

*Logs abrufen:*

```bash
docker logs m169_logtest
```

*Erwartete Ausgabe:*

```bash
Hallo
Ciao
```

### Wechsel des Log-Drivers auf syslog und Logs überwachen

*Container mit syslog-Log-Driver starten:*

```bash
docker run -d --log-driver=syslog --name m169_syslogtest ubuntu bash -c 'i=0; while true; do i=$((i+1)); echo "docker $i"; sleep 1; done;'
```

*Logs in Echtzeit überwachen:*

```bash
tail -f /var/log/syslog
```

### Anzahl der ausgegebenen Log-Zeilen zählen

*Logzeilen mit Filter auf Container-Namen zählen:*

```bash
grep m169_syslogtest /var/log/syslog | wc -l
```

### Container stoppen und entfernen

*Container stoppen:*

```bash
docker stop m169_syslogtest
```

*Container entfernen:*

```bash
docker rm m169_syslogtest
```

## Quellen

Gitlab Repository von Herr Rohr 
