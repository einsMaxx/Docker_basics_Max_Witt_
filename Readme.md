# Docker Grundlagen

In diesem Repository sind verschiedene Docker Container.

Ich habe Docker Desktop installiert und die Container mit Docker Compose gestartet.

## Wichtige Befehle

Mit diesem Befehl kann man sehen, welche Container gerade laufen:

```bash
docker ps
```

Mit diesem Befehl kann man alle Container sehen, auch die gestoppten:

```bash
docker ps -a
```

## Pi-hole

Pi-hole ist ein DNS-Server. Er kann Werbung und unerwünschte Webseiten im Netzwerk blockieren.

Starten:

```bash
docker compose -f pihole/pihole.yml up -d
```

Die Webseite von Pi-hole ist erreichbar über:

```text
http://localhost/admin
```

## Portainer

Portainer ist eine Weboberfläche für Docker. Dort kann man Container sehen, starten und stoppen.

Starten:

```bash
docker compose -f portainer/portainer.yml up -d
```

Die Webseite von Portainer ist erreichbar über:

```text
http://localhost:9000
```

## Watchtower

Watchtower sucht nach Updates für Docker Container. Wenn es ein Update gibt, kann Watchtower den Container automatisch aktualisieren.

Starten:

```bash
docker compose -f watchtower/watchtower.yml up -d
```

Watchtower hat keine eigene Webseite. Den Status kann man mit Docker Desktop oder mit diesem Befehl sehen:

```bash
docker logs watchtower
```

## Nginx

Nginx ist ein Webserver. Er zeigt eine einfache Webseite an.

Starten:

```bash
docker compose -f nginx/nginx.yml up -d
```

Die Webseite von Nginx ist erreichbar über:

```text
http://localhost:8080
```

## Docker Desktop

In Docker Desktop kann man alle Container sehen. Dort kann man die Container auch starten und stoppen.