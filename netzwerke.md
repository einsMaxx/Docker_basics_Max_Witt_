# Netzwerke mit Docker Compose

Quelle: [Docker Dokumentation - Networking in Compose](https://docs.docker.com/compose/how-tos/networking/)

## 1. Standardnetzwerk

Wenn kein Netzwerk angegeben ist, erstellt Docker Compose ein Bridge-Netzwerk mit dem Namen:

```text
projektname_default
```

Alle Services aus der Compose-Datei werden mit diesem Netzwerk verbunden.

## 2. Verbindung über Servicenamen

Container im gleichen Netzwerk können sich über den Servicenamen erreichen. Ein Service mit dem Namen `db` ist zum Beispiel über `db` erreichbar.

Die IP-Adresse sollte nicht benutzt werden. Docker kann einem Container beim Neustart eine neue IP-Adresse geben. Der Servicename bleibt gleich.

## 3. Eigenes Netzwerk

Ein eigenes Netzwerk wird unten in der Compose-Datei unter `networks` erstellt. Beim Service wird es ebenfalls unter `networks` eingetragen.

```yaml
services:
  nginx:
    networks:
      - lab_net

networks:
  lab_net:
    driver: bridge
```

## 4. network_mode

Mögliche Werte sind:

- `host`: Der Container benutzt direkt das Netzwerk vom PC.
- `none`: Der Container hat kein Netzwerk.
- `service:name`: Der Container benutzt das Netzwerk von einem anderen Service.
- `container:name`: Der Container benutzt das Netzwerk eines bestimmten Containers.

`host` eignet sich zum Beispiel für Netzwerküberwachung. Der Container kann dann direkt auf die Netzwerkkarten und Ports vom PC zugreifen. Portweiterleitungen sind dabei nicht möglich. Außerdem funktioniert die Auflösung über Servicenamen nicht.

## 5. stop und down

```bash
docker compose stop
```

Stoppt nur die Container. Die Container und Netzwerke bleiben erhalten.

```bash
docker compose down
```

Stoppt und entfernt die Container. Netzwerke, die von Compose erstellt wurden, werden ebenfalls entfernt.

Ein Netzwerk mit `external: true` wird nicht entfernt. Es wurde nicht von Compose erstellt und kann auch von anderen Projekten benutzt werden.

## 6. Mehrere Compose-Projekte

Zwei unterschiedliche Compose-Projekte können über ein gemeinsames externes Netzwerk verbunden werden.

```bash
docker network create inter-project
```

In beiden Compose-Dateien wird dieses Netzwerk dann als `external: true` eingetragen. Die Services können sich danach über ihre Servicenamen erreichen.

## 7. Netzwerk-Aliase

Ein Alias ist ein zusätzlicher Name für einen Service im Netzwerk. So kann ein Container unter mehreren Namen erreichbar sein.

```yaml
services:
  nginx:
    networks:
      lab_net:
        aliases:
          - webserver
```

Der Service `nginx` ist dann zum Beispiel über `nginx` und `webserver` erreichbar.

## 8. IP-Adressen

Docker vergibt IP-Adressen normalerweise automatisch. Sie können sich nach einem Neustart ändern.

Statische IP-Adressen können mit `ipam`, `subnet` und `ipv4_address` festgelegt werden:

```yaml
services:
  nginx:
    networks:
      lab_net:
        ipv4_address: 172.28.0.10

networks:
  lab_net:
    ipam:
      config:
        - subnet: 172.28.0.0/16
```

In dieser Aufgabe werden keine statischen IP-Adressen verwendet. Die Services sollen über ihre Servicenamen erreichbar sein.

## 9. Host-Port und Container-Port

Bei einer Portweiterleitung wie `8080:80` ist:

- `8080` der Host-Port auf dem PC
- `80` der Container-Port im Container

Von außen wird der Host-Port verwendet, zum Beispiel `http://localhost:8080`. Für die Verbindung zwischen Containern wird der Container-Port verwendet.

## Praktischer Test

Die Compose-Datei wurde zuerst mit folgendem Befehl geprüft:

```bash
docker compose -f compose.yml config
```

Danach wurden alle Services gemeinsam gestartet:

```bash
docker compose -f compose.yml up -d
docker compose -f compose.yml ps
docker ps
docker network inspect lab_net
docker compose -f compose.yml exec nginx getent hosts pihole
```

Ergebnis:

- Alle vier Container konnten gestartet werden.
- Pi-hole, Portainer, Watchtower und Nginx waren mit `lab_net` verbunden.
- `lab_net` wurde mit dem Treiber `bridge` erstellt.
- Nginx konnte den Servicenamen `pihole` auflösen.
- Es gab keine Portkonflikte.
- Watchtower benötigte für die aktuelle Docker-Version die Einstellung `DOCKER_API_VERSION: "1.40"`. Danach lief der Container fehlerfrei.

Zum Beenden wird folgender Befehl verwendet:

```bash
docker compose -f compose.yml down
```
