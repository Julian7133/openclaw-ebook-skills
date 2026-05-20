# Kapitel 14: Workflow 4 - Das Morgen-Briefing

Diese Seite ist der schnelle Einstieg zum Morgen-Briefing. Der Skill selbst liegt im Ordner [`../skills/daily-briefing/`](../skills/daily-briefing/). Hier finden Sie die Befehle, Prompts und den Cron-Block aus dem Kapitel.

Das Morgen-Briefing bündelt Termine, wichtige E-Mails und fällige Leads zu einem kurzen Tagesüberblick.

## 1. Begleit-Repo aktualisieren

Wenn das Repo schon vorhanden ist:

```bash
cd ~/.openclaw/workspace/begleit-repo
git pull
```

Wenn Sie es noch nicht geklont haben:

```bash
cd ~/.openclaw/workspace
git clone https://github.com/Julian7133/openclaw-ebook-skills.git begleit-repo
```

## 2. Skill in OpenClaw kopieren

```bash
cd ~/.openclaw/workspace
mkdir -p skills
cp -r begleit-repo/de/skills/daily-briefing skills/
```

## 3. OpenClaw neu starten

```bash
openclaw gateway restart
```

Starten Sie danach eine neue OpenClaw-Session.

## 4. Optional: Heartbeat schneller machen

Im Kapitel wird empfohlen, die laufenden Checks für E-Mail und Kalender von 90 Minuten auf 30 Minuten zu verkürzen, wenn Sie morgens ein zuverlässigeres Briefing möchten.

Kopieren Sie dafür diesen Prompt in OpenClaw, nicht ins Terminal:

```text
Bitte prüfe meine HEARTBEAT.md. Wenn dort E-Mail-Triage oder Kalender-Check ungefähr alle 90 Minuten laufen, ändere den Rhythmus auf ungefähr alle 30 Minuten. Behalte die Filterregeln bei: keine Newsletter, keine Systemmails, keine unwichtigen Termine. Zeige mir vorher kurz, welche Zeilen du ändern willst.
```

## 5. Morgen-Briefing manuell testen

```text
Erstelle mein Morgen-Briefing für heute. Nutze meine heutigen Termine, wichtige E-Mails seit Mitternacht und - falls heute Montag ist - überfällige Leads. Halte die Ausgabe kurz und biete mir am Ende konkrete Aktionen an.
```

Wenn noch keine echten Daten vorhanden sind, darf das Briefing Lücken melden. Wichtig ist, dass der Skill nicht abbricht.

## 6. Täglichen Cron eintragen

Verwenden Sie diesen JSON-Block als Vorlage in `~/.openclaw/cron/jobs.json`:

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
    "message": "Nutze den daily-briefing-Skill und erstelle das Morgen-Briefing fuer heute."
  },
  "delivery": {
    "mode": "announce",
    "to": "IHRE_TELEGRAM_CHAT_ID"
  }
}
```

Wenn Sie OpenClaw lieber die Datei bearbeiten lassen möchten, kopieren Sie diesen Prompt:

```text
Lege in meiner Cron-Konfiguration ein Morgen-Briefing an. Es soll montags bis freitags um 07:00 im Main-Kontext laufen, den daily-briefing-Skill nutzen und mir eine kompakte Tagesübersicht mit Aktionen posten. Bitte zeige mir den JSON-Block, bevor du ihn speicherst.
```

## 7. Cron-Dienst neu laden

```bash
openclaw gateway restart
```

## Dateien zum Nachsehen

- [`../skills/daily-briefing/SKILL.md`](../skills/daily-briefing/SKILL.md) - Hauptregeln des Skills
- [`../skills/daily-briefing/references/briefing-format.md`](../skills/daily-briefing/references/briefing-format.md) - Ausgabeformat
- [`../skills/daily-briefing/references/meeting-prep-rules.md`](../skills/daily-briefing/references/meeting-prep-rules.md) - Regeln für Terminvorbereitung
