# Kapitel 14: Workflow 4 — Das Morgen-Briefing

## Kalender-Heartbeat anpassen

```text
In der HEARTBEAT.md steht aktuell, dass externe Termine 90 Minuten vorher gemeldet werden. Ändere das auf 30 Minuten. Alle anderen Regeln aus 12.6 bleiben gleich.
```

## Skill installieren

```bash
cd ~/.openclaw/workspace/begleit-repo
git pull
mkdir -p ~/.openclaw/workspace/skills
cp -r de/skills/daily-briefing ~/.openclaw/workspace/skills/
openclaw gateway restart
```

## Cron bearbeiten

```bash
nano ~/.openclaw/cron/jobs.json
```

## Morgen-Briefing

```json
{
  "agentId": "main",
  "name": "Morgen-Briefing",
  "enabled": true,
  "schedule": {
    "kind": "cron",
    "expr": "0 7 * * 1-5",
    "tz": "Europe/Berlin"
  },
  "sessionTarget": "main",
  "payload": {
    "kind": "systemEvent",
    "message": "Nutze den daily-briefing-Skill und erstelle das Morgen-Briefing für heute."
  },
  "delivery": {
    "mode": "announce",
    "to": "IHRE_TELEGRAM_CHAT_ID"
  }
}
```

## Manuell testen

```text
Gib mir bitte mein Morgen-Briefing.
```

```text
Briefing bitte.
```

## Folgeaktion ausführen

```text
Ja, bereite bitte das Schumann-Gespräch vor.
```

## Interne Termine zusammenfassen

```text
Im Briefing sollen interne Meetings nur als Zusammenfassung auftauchen, nicht einzeln. Externe Termine bleiben wie jetzt mit Prep.
```

## Lead-Abschnitt erweitern

```text
Zeige den Lead-Abschnitt an Montag, Dienstag und Mittwoch, nicht nur montags.
```

## Reklamations-Aktionen anpassen

```text
Wenn du bei Reklamations-Mails einen Antwortentwurf erzeugst, biete im Entwurf immer auch einen Telefon-Rückruf für heute oder morgen an. Nicht nur auf die Mail textlich antworten.
```
