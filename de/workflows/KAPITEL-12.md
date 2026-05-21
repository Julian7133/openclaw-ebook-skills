# Kapitel 12: Workflow 2 — Der Kalender-Assistent

## Master-Prompt für OpenClaw

Kopiere diesen Prompt und sende ihn an deinen OpenClaw-Assistenten:

```
Richte meinen Kalender-Assistenten ein.

Hier meine Daten:  
- CalDAV-URL: [DEINE_CALDAV_URL_HIER]
- Benutzername (E-Mail): [DEINE_EMAIL]
- App-Passwort: [DEIN_APP_PASSWORT]

Bitte:  
1. Erstelle ~/.config/vdirsyncer/config mit der richtigen CalDAV-URL und Benutzername.  
2. Erstelle ~/.config/vdirsyncer/password mit dem App-Passwort und setze chmod 600.  
3. Erstelle ~/.config/khal/config mit deutscher Zeitzone.  
4. Führe `vdirsyncer discover` und `vdirsyncer sync` aus.  
5. Erweitere HEARTBEAT.md um einen Kalender-Check.  
6. Schlage mir einen Cron-Job für `vdirsyncer sync` alle 15 Minuten vor.  
7. Fordere mich auf, das App Passwort manuell durch nano zu setzen.

Zeige mir die nächsten 7 Tage mit `khal list today 7d`.
```

**Hinweis:** Diesen Prompt kannst du auch an **Claude Code** senden (falls du Variante B aus Kapitel 4 nutzt).

## Vorbereitung: Tools installieren (einmalig)

```bash
# Nur dieser Befehl muss manuell ausgeführt werden (sudo benötigt)
sudo apt update && sudo apt install -y vdirsyncer khal
```

## CalDAV URLs gängiger Anbieter

```
IONOS Mail Business: https://dav.mailbusiness.ionos.com/
Strato: https://caldav.strato.de/
Nextcloud: https://ihre-nextcloud.de/remote.php/dav/
mailbox.org: https://dav.mailbox.org/
iCloud: https://caldav.icloud.com/ (eigene URL pro Account)
```

## Manuelle Einrichtung (alternativ)

```bash
# Passwort manuell setzen
nano ~/.config/vdirsyncer/password
# Dein App-Passwort einfügen und speichern
chmod 600 ~/.config/vdirsyncer/password

# Crontab für automatische Synchronisation
crontab -e
# Diese Zeile hinzufügen (sync alle 15 Minuten):
*/15 * * * * /usr/bin/vdirsyncer sync
```

## HEARTBEAT.md Kalender-Check

Dein Assistent wird diesen Abschnitt automatisch hinzufügen:

```markdown
2. [Kalender-Check] Alle 1 Stunde: Termine der nächsten 24h prüfen.

A) EXTERNE TERMINE (location != ""):
   -> Meldung 90 Min vor Beginn.
   -> Format: "In 90 Min: [Titel] ([Ort])."

B) TERMINKONFLIKT (zwei Termine überschneiden sich):
   -> Sofortige Meldung.
   -> Format: "Konflikt: [Titel 1] und [Titel 2]."

C) ENGER PUFFER zwischen externen Terminen:
   -> Wenn zwischen Ende Termin N und Anfang Termin N+1 < 45 Min liegen.
   -> Meldung einmalig am Vorabend um 18:00 Uhr.

D) NEU HINZUGEKOMMENE TERMINE in den nächsten 3h:
   -> Sofortige Meldung.
   -> Format: "Kurzfristig eingeschoben: [Titel] um [Zeit]."
```

## Test: Kalender anzeigen

```bash
# Termine der nächsten 7 Tage anzeigen
khal list today 7d

# Heutige Termine anzeigen
khal list today

# Synchronisation manuell starten
vdirsyncer sync
```

## Gateway neu starten

```bash
openclaw gateway restart
```

---

**Detaillierte Konfigurationen und Anpassungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides