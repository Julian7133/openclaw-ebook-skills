# Kapitel 13: Workflow 3 — Lead-Follow-Up

## Skill installieren

```bash
cd ~/.openclaw/workspace
git clone https://github.com/Julian7133/openclaw-ebook-skills.git begleit-repo
mkdir -p skills
cp -r begleit-repo/de/skills/lead-tracker skills/
openclaw gateway restart
```

## Skill aktualisieren

```bash
cd ~/.openclaw/workspace/begleit-repo
git pull
cp -r de/skills/lead-tracker ~/.openclaw/workspace/skills/
openclaw gateway restart
```

## Ersten Lead eintragen

```text
Bitte trag mir einen Lead ein: Hans Fischer von Fischer-Werke KG. Er hat im Februar wegen einer Solaranlage angefragt, ich habe ihm ein Angebot geschickt. Er will erst nach Q2 entscheiden.
```

```text
Offen. Mail: hans.fischer@fischer-werke.de
```

## Lead-Anzahl prüfen

```text
Wie viele Leads habe ich jetzt?
```

## Posteingang scannen

```text
Scan bitte meinen Posteingang der letzten 30 Tage und zeig mir, welche Absender Lead-Kandidaten sein könnten.
```

## Kandidat aufnehmen

```text
Ja. Nächster Schritt: Angebot kalkulieren, Rückmeldung bis 25.03.
```

## Cron bearbeiten

```bash
nano ~/.openclaw/cron/jobs.json
```

## Montag-Review

```json
{
  "agentId": "main",
  "name": "Lead-Review",
  "enabled": true,
  "schedule": {
    "kind": "cron",
    "expr": "0 8 * * 1",
    "tz": "Europe/Berlin"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Nutze den lead-tracker-Skill und führe einen Review durch."
  },
  "delivery": {
    "mode": "announce",
    "to": "IHRE_TELEGRAM_CHAT_ID"
  }
}
```

## Lead-Typen anpassen

```text
Die 14-Tage-Regel passt nicht zu meinem Geschäft. Ich habe drei Lead-Typen: schnell (3 Tage Frist), normal (14 Tage) und langsam (60 Tage). Bitte baue das ein und geh meine bestehenden Leads durch — die Großaufträge sind langsam, Austauschteile sind schnell, alles andere normal.
```

## Wartezeit einbauen

```text
Wenn ich entscheide, dass ich einen Lead bewusst liegen lasse, will ich ein Datum eintragen können, ab dem du mich wieder erinnerst. Und bis dahin soll der Lead nicht im Montag-Review auftauchen. Bitte baue das ein.
```

## Manuell testen

```text
Fasse meine offenen Leads nach Dringlichkeit zusammen und schreib mir einen Vorschlag für die erste Follow-Up-Mail an Hans Fischer.
```
