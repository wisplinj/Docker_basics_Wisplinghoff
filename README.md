# Hier alle im Video erwähnten Befehle:

## Docker installieren:

```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
```

Falls kein curl installiert: `apt install curl`

Wenn man nicht mit dem Nutzer Root arbeitet, sollte man den aktuellen Benutzer berechtigen:

```
sudo usermod -aG docker $USER
```

## Mit Docker arbeiten

* Laufende Container auflisten: `docker ps`
* Alle Container auflisten (auch gestoppte): `docker ps -a`
* Einen Container anhalten: `docker stop <Containername>` (den Namen findet man mit `docker ps` heraus)
* Einen gestoppten Container endgültig löschen: `docker rm <Containername>`

## Einen simplen Webserver starten

Der Container aus dem Image nginx fährt mit folgendem Befehl hoch:

```
docker run -p 80:80 nginx
```
Die eigene IP-Adresse erhält man mit `ip a`

## Arbeiten mit Docker-Compose

Legt euch am besten einen eigenen Ordner für das Docker-Projekt an, um Ordnung zu halten. Die Datei docker-compose.yml enthält die Definition der Container.

Bearbeitet wird die Datei mit:

```
nano docker-compose.yml
```

Die Inhalte findet ihr unten in diesem GitHub-Gist. Den Texteditor Nano beendet man mit: `Strg+X`, dann `Y`

Die Compose-Zusammenstellung hochfahren:

```
docker compose up -d
```

Will man die Container updaten, lädt man die neuen Images mit 

```
docker compose pull
```

## Eine oder mehrere Docker-Compose-Dateien?

Das ist definitiv Geschmachssache und hängt von der Umgebung ab. Wenn man mehr als ein Projekt (zum Beispiel einen Blog und ein Pihole) auf einem Server betreibt, sollte man für jedes einen Ordner anlegen und darin eine Docker-Compose-Datei ablegen. Die nützlichen Helfer wie Portainer und Watchtower kommen zusammen in eine weitere Datei. Dann kann man mit `docker compose down`gezielt Teile der Umgebung herunterfahren.





# Docker Basics – Übersicht der verwendeten Container

In diesem Projekt wurden mehrere Docker-Container eingerichtet und mit Docker Compose gestartet.  
Im Folgenden werden die einzelnen Dienste kurz erklärt.

---

## 🟩 Pi-hole
**Beschreibung:**  
Pi-hole ist ein DNS-Filter, der Werbung und Tracker im gesamten Netzwerk blockiert.  
Er fungiert als lokaler DNS-Server und filtert unerwünschte Domains heraus.

**Weboberfläche:**  
http://localhost:8081/admin

**Besonderheiten:**  
- Gute Übersicht über geblockte Anfragen  
- Kann als Netzwerkschutz für alle Geräte genutzt werden  

---

## 🟦 Portainer
**Beschreibung:**  
Portainer ist eine grafische Benutzeroberfläche, mit der man Docker verwalten kann.  
Container lassen sich damit starten, stoppen, ansehen und konfigurieren.

**Weboberfläche:**  
http://localhost:9000

**Besonderheiten:**  
- Sehr hilfreich, um Docker ohne Terminal zu steuern  
- Zeigt Logs, Container, Volumes, Netzwerke usw.

---

## 🟨 Watchtower
**Beschreibung:**  
Watchtower überwacht laufende Docker-Container und aktualisiert sie automatisch,  
wenn neue Versionen der Images verfügbar sind.

**Weboberfläche:**  
Keine – Watchtower läuft im Hintergrund.

**Logs ansehen:**  
```
docker logs <watchtower-container-name>
```

**Besonderheiten:**  
- Automatische Updates  
- Weniger manuelle Pflege nötig  

---

## 🟥 Nginx
**Beschreibung:**  
Nginx ist ein schneller, schlanker Webserver.  
In diesem Projekt liefert er eine eigene `index.html` aus dem Ordner `nginx/html` aus.

**Weboberfläche:**  
http://localhost:8080

**Besonderheiten:**  
- Perfekt für statische Webseiten  
- Sehr leichtgewichtig und weit verbreitet

---

# 🧩 Starten der Container

```
docker compose -f pihole/pihole.yml up -d
docker compose -f portainer/portainer.yml up -d
docker compose -f watchtower/watchtower.yml up -d
docker compose -f nginx/nginx.yml up -d
```

# 🛑 Stoppen der Container

```
docker compose -f pihole/pihole.yml down
docker compose -f portainer/portainer.yml down
docker compose -f watchtower/watchtower.yml down
docker compose -f nginx/nginx.yml down
```
