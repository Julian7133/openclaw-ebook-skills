# Kapitel 11: Workflow 1 — E-Mail-Triage

## Master-Prompt für OpenClaw

Kopiere diesen Prompt und sende ihn an deinen OpenClaw-Assistenten:

```
Richte mein E-Mail-Triage-System ein.

Hier meine Daten:
- E-Mail-Adresse: [DEINE_EMAIL_HIER]
- App-Passwort: [DEIN_APP_PASSWORT_HIER]
- IMAP-Server: imap.ionos.de:993 (TLS)
- SMTP-Server: smtp.ionos.de:587 (STARTTLS)

Bitte:
1. Installiere Himalaya mit dem offiziellen Install-Script.
2. Erstelle ~/.config/himalaya/config.toml mit den obigen Werten.
3. Speichere das App-Passwort in ~/.config/himalaya/password mit chmod 600.
4. Teste mit `himalaya envelopes list` und zeige mir die Ausgabe.
5. Erweitere HEARTBEAT.md um einen E-Mail-Check (alle 3 Stunden).
6. Fordere mich auf, das App-Passwort manuell durch nano zu setzen.
```

## Master-Prompt für Claude Code

Wenn du mit Claude Code arbeitest (Variante B aus Kapitel 4):

```
Rolle: Du bist Senior-Systemadministrator. Ich habe OpenClaw bereits auf einem Ubuntu-24.04-Hetzner-Server installiert.

Aufgabe: Richte Himalaya auf diesem System ein und verbinde es mit meinem IONOS-Postfach.

Meine Daten: IMAP: imap.ionos.de:993 (TLS). SMTP: smtp.ionos.de:587 (STARTTLS). Konto: meine-adresse@meine-domain.de. App-Passwort liegt in der Datei: [Pfad einfügen]

Schritte: 1) Prüfe, ob Himalaya installiert ist; falls nicht, installiere es über das offizielle Install-Script. 2) Lege ~/.config/himalaya/config.toml an mit obigen Werten. 3) Speichere das App-Passwort in ~/.config/himalaya/password mit chmod 600. 4) Teste mit „himalaya envelopes list" und zeige mir die Ausgabe.
```

## Manuelle Einrichtung (alternativ)

```bash
# Himalaya installieren
curl -sSL https://raw.githubusercontent.com/pimalaya/himalaya/master/install.sh | sudo sh

# Version prüfen
himalaya --version

# Konfigurationsordner erstellen
mkdir -p ~/.config/himalaya

# Config Datei erstellen
nano ~/.config/himalaya/config.toml
```

## Himalaya config.toml Template (IONOS Beispiel)

```toml
[accounts.default]
default = true
email = "DEIN_KONTO@DEINE_DOMAIN.de"

backend.type = "imap"
backend.host = "imap.ionos.de"
backend.port = 993
backend.encryption = "tls"
backend.login = "DEIN_KONTO@DEINE_DOMAIN.de"
backend.auth.type = "password"
backend.auth.cmd = "cat ~/.config/himalaya/password"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.ionos.de"
message.send.backend.port = 587
message.send.backend.encryption = "start-tls"
message.send.backend.login = "DEIN_KONTO@DEINE_DOMAIN.de"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "cat ~/.config/himalaya/password"
```

## App-Passwort speichern

```bash
# Passwort in Datei speichern
echo "DEIN_APP_PASSWORT" > ~/.config/himalaya/password
chmod 600 ~/.config/himalaya/password

# Oder manuell setzen
nano ~/.config/himalaya/password
# Dein Passwort einfügen, dann Strg+O, Enter, Strg+X
```

## Test: E-Mails abrufen

```bash
# Letzte Nachrichten auflisten
himalaya envelopes list

# Nur Seite 1
himalaya envelopes list -p 1

# Alle Ordner anzeigen
himalaya folder list

# Einzelne Nachricht lesen
himalaya message read [NACHRICHT_ID]
```

## HEARTBEAT.md E-Mail-Check hinzufügen

Dein Assistent wird diesen Abschnitt automatisch hinzufügen:

```markdown
1. [E-Mail-Check] Alle 3 Stunden: Prüfe ungelesene Nachrichten
   im INBOX-Ordner der letzten 3 Stunden.
   
   Nutze dafür: himalaya envelopes list --folder INBOX
   
   Filter:
   - Nur Nachrichten mit ungelesener Markierung
   - Ignoriere Absender: *@newsletter.*, noreply@*, no-reply@*
   - Ignoriere Betreffzeilen mit: "Newsletter", "Abonnement"
   
   Meldeschwelle: Nur Rückmeldung, wenn mindestens
   eine Nachricht die Filter passiert.
   
   Antwortformat:
   "X neue wichtige E-Mails seit [Uhrzeit]:
   - [Absender]: [Betreff]"
```

## Gateway neu starten

```bash
openclaw gateway restart
```

---

**Anbieter-spezifische Einstellungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides