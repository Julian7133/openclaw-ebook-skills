# Kapitel 12: Workflow 2 - Der Kalender-Assistent

Diese Seite ist die Kopiervorlage zum Kapitel. Sie verbindet OpenClaw mit Ihrem Kalender, damit der Assistent Termine lesen und rechtzeitig auf Vorbereitung oder Konflikte hinweisen kann.

Kopieren Sie die Befehle Schritt für Schritt in Ihr Server-Terminal.

## 1. Kalender-Werkzeuge installieren

```bash
sudo apt update
sudo apt install -y vdirsyncer khal
```

Prüfen Sie danach, ob beide Befehle verfügbar sind:

```bash
vdirsyncer --version
khal --version
```

## 2. Konfigurationsordner anlegen

```bash
mkdir -p ~/.config/vdirsyncer ~/.config/khal ~/.calendars
chmod 700 ~/.config/vdirsyncer ~/.config/khal ~/.calendars
```

## 3. CalDAV-Konfiguration anlegen

Ersetzen Sie die Beispielwerte durch Ihre echten Daten:

- `https://caldav.example.com/remote.php/dav/calendars/ihr-name/` durch Ihre CalDAV-Adresse
- `ihr-name` durch Ihren Benutzernamen
- `pass show caldav` durch Ihren eigenen Passwortbefehl, falls Sie einen anderen Eintrag nutzen

```bash
cat > ~/.config/vdirsyncer/config <<'EOF'
[general]
status_path = "~/.vdirsyncer/status/"

[pair hauptkalender]
a = "hauptkalender_remote"
b = "hauptkalender_local"
collections = ["from a", "from b"]
metadata = ["displayname", "color"]

[storage hauptkalender_remote]
type = "caldav"
url = "https://caldav.example.com/remote.php/dav/calendars/ihr-name/"
username = "ihr-name"
password.fetch = ["command", "pass", "show", "caldav"]

[storage hauptkalender_local]
type = "filesystem"
path = "~/.calendars/"
fileext = ".ics"
EOF
chmod 600 ~/.config/vdirsyncer/config
```

### Häufige CalDAV-Anbieter

Nutzen Sie die Angaben Ihres Anbieters. Diese Beispiele zeigen nur, welche Art von URL gesucht ist:

```text
Nextcloud: https://ihre-domain.example/remote.php/dav/calendars/ihr-name/
Mailbox.org: https://dav.mailbox.org/caldav/
Fastmail: https://caldav.fastmail.com/
Google Kalender: meist über App-Passwort oder Export/Bridge, nicht direkt mit normalem Google-Passwort
```

## 4. Passwort sicher hinterlegen

Speichern Sie das Kalender-Passwort nicht direkt in der Datei. Wenn Sie `pass` bereits eingerichtet haben:

```bash
pass insert caldav
```

Wenn nicht, kopieren Sie diesen Prompt in OpenClaw:

```text
Hilf mir, den Passwortspeicher `pass` für meinen CalDAV-Kalender einzurichten. Ziel: `vdirsyncer` soll das Passwort über `pass show caldav` lesen können. Erkläre jeden Schritt kurz und frage nach, bevor du bestehende Dateien änderst.
```

## 5. Kalender synchronisieren

```bash
vdirsyncer discover
vdirsyncer sync
```

Wenn mehrere Kalender gefunden werden, bestätigen Sie nur die Kalender, die OpenClaw wirklich kennen soll.

## 6. Khal konfigurieren

```bash
cat > ~/.config/khal/config <<'EOF'
[calendars]

[[hauptkalender]]
path = ~/.calendars/*
type = discover

[locale]
timeformat = %H:%M
dateformat = %d.%m.%Y
longdateformat = %d.%m.%Y
datetimeformat = %d.%m.%Y %H:%M
longdatetimeformat = %d.%m.%Y %H:%M
EOF
chmod 600 ~/.config/khal/config
```

## 7. Kalenderzugriff testen

```bash
khal list today
khal list tomorrow
khal list now 7d
```

Wenn Termine erscheinen, ist der Kalender bereit.

## 8. Kalender-Assistent in OpenClaw testen

Kopieren Sie diesen Prompt in OpenClaw:

```text
Lies mit `khal list now 7d` meine Termine der nächsten sieben Tage. Suche nach Terminen mit Ort, Kundennamen oder längerer Beschreibung. Zeige mir nur Termine, bei denen Vorbereitung sinnvoll sein könnte, und schlage maximal drei konkrete Vorbereitungsschritte vor.
```

## 9. HEARTBEAT.md ergänzen

Kopieren Sie diesen Prompt in OpenClaw, damit der Assistent die regelmäßige Kalenderprüfung einträgt:

```text
Ergänze meine HEARTBEAT.md um einen Kalender-Check. Prüfe ungefähr alle 90 Minuten mit `vdirsyncer sync` und `khal list now 48h`, ob in den nächsten 48 Stunden neue oder geänderte Termine mit Vorbereitungsbedarf auftauchen. Melde nur relevante Änderungen, nicht jeden normalen Termin.
```

## 10. Gateway neu starten

```bash
openclaw gateway restart
```

## Wenn etwas nicht klappt

Nutzen Sie diesen Prompt in OpenClaw:

```text
Meine Kalender-Einrichtung mit vdirsyncer/khal funktioniert noch nicht. Bitte analysiere die Fehlermeldung, unterscheide zwischen Login-Problem, falscher CalDAV-URL und lokaler Konfiguration, und gib mir den nächsten sicheren Testbefehl.
```
