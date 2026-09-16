# Docker Grundlagen

In diesem Repository sind vier Docker-Compose-Dateien für Pi-hole, Portainer, Watchtower und Nginx.

Die Grundlagen stammen aus dem [c’t-3003-Video](https://www.youtube.com/watch?v=LBG51Gygg7A) und dem [zugehörigen GitHub-Gist](https://gist.github.com/jamct/2e6c03f60319423bc4bc6c23fc0aa359).

## Wichtige Befehle

- Laufende Container anzeigen:

```bash
docker ps
```

- Alle Container anzeigen:

```bash
docker ps -a
```

## Pi-hole

Pi-hole ist ein DNS-Server. Er kann Werbung und unerwünschte Domains im Netzwerk blockieren.

- Image: `pihole/pihole:latest`
- Ports: 53 für DNS und 80 für die Weboberfläche
- Webseite: [http://localhost/admin](http://localhost/admin)

Starten:

```bash
docker compose -f pihole/pihole.yml up -d
```

Beenden:

```bash
docker compose -f pihole/pihole.yml down
```

Beobachtung: Pi-hole wurde gestartet und mit `docker ps` kontrolliert. Die Weboberfläche war unter `http://localhost/admin` erreichbar.

## Portainer

Portainer ist eine Weboberfläche zum Verwalten von Docker-Containern.

- Image: `portainer/portainer-ce:latest`
- Port: 9000
- Webseite: [http://localhost:9000](http://localhost:9000)

Starten:

```bash
docker compose -f portainer/portainer.yml up -d
```

Beenden:

```bash
docker compose -f portainer/portainer.yml down
```

Beobachtung: Portainer wurde gestartet und über die Weboberfläche ein Administratorkonto eingerichtet.

## Watchtower

Watchtower prüft Docker-Container auf neue Images und kann sie automatisch aktualisieren.

- Image: `containrrr/watchtower:latest`
- Keine Weboberfläche vorhanden
- Kontrolle über `docker ps` und `docker logs watchtower`

Starten:

```bash
docker compose -f watchtower/watchtower.yml up -d
```

Beenden:

```bash
docker compose -f watchtower/watchtower.yml down
```

Beobachtung: Der Container wurde gestartet. Die Logausgabe zeigte auf dem aktuellen Docker Desktop einen Fehler wegen einer zu alten Docker-API der Watchtower-Version aus dem Video.

## Nginx

Nginx ist ein Webserver. Er zeigt eine einfache Webseite an.

- Image: `nginx:latest`
- Port: 8080 auf dem PC wird auf Port 80 im Container weitergeleitet
- Webseite: [http://localhost:8080](http://localhost:8080)

Starten:

```bash
docker compose -f nginx/nginx.yml up -d
```

Beenden:

```bash
docker compose -f nginx/nginx.yml down
```

Beobachtung: Nginx wurde gestartet. Die Standardseite war unter `http://localhost:8080` erreichbar. Der Container wurde außerdem in Docker Desktop gestoppt und wieder gestartet.

## Docker Desktop

Die Container wurden auch in Docker Desktop angezeigt. Dort können sie gestartet und gestoppt werden. Den Status kann man danach mit `docker ps` oder `docker ps -a` kontrollieren.
