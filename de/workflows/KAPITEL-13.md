# Kapitel 13: Workflow 3 - Lead-Follow-Up

Diese Seite ist der schnelle Einstieg zum Lead-Tracker. Der Skill selbst liegt im Ordner [`../skills/lead-tracker/`](../skills/lead-tracker/). Hier finden Sie nur die Befehle und Test-Prompts aus dem Kapitel.

Der Lead-Tracker führt eine einfache Liste offener Verkaufschancen und erinnert Sie an sinnvolle Follow-Ups.

## 1. Begleit-Repo holen oder aktualisieren

Wenn Sie das Repo noch nicht geklont haben:

```bash
cd ~/.openclaw/workspace
git clone https://github.com/Julian7133/openclaw-ebook-skills.git begleit-repo
```

Wenn das Repo schon vorhanden ist:

```bash
cd ~/.openclaw/workspace/begleit-repo
git pull
```

## 2. Skill in OpenClaw kopieren

```bash
cd ~/.openclaw/workspace
mkdir -p skills
cp -r begleit-repo/de/skills/lead-tracker skills/
```

## 3. OpenClaw neu starten

```bash
openclaw gateway restart
```

Starten Sie danach eine neue OpenClaw-Session, damit der neue Skill geladen wird.

## 4. Ersten Lead anlegen

Kopieren Sie diesen Prompt in OpenClaw. Er ist absichtlich konkret, damit Sie sofort sehen, ob der Skill schreibt.

```text
Trage einen neuen Lead ein: Anna Müller von der Beispiel GmbH, E-Mail anna.mueller@example.com. Status offen. Letzter Kontakt war heute. Nächster Schritt: Angebot nachfassen. Notiz: Interessiert sich für eine Automatisierung der Angebotsprüfung.
```

Die Beispiel GmbH ist fiktiv. Ersetzen Sie sie später durch echte Kontakte.

## 5. Anzahl prüfen

```text
Wie viele Leads habe ich aktuell? Bitte gruppiere nach Status.
```

## 6. Follow-Up-Review testen

```text
Welche Leads sind überfällig? Nutze die Review-Regeln aus dem Lead-Tracker und zeige mir die wichtigsten nächsten Schritte.
```

## 7. Wöchentlichen Review als Cron eintragen

Kopieren Sie diesen Prompt in OpenClaw:

```text
Lege einen wöchentlichen Cron-Job für den Lead-Tracker an. Jeden Montag um 08:15 soll OpenClaw prüfen, welche Leads überfällig sind. Der Lauf darf in einer isolierten Session stattfinden und soll mir nur eine kurze Zusammenfassung mit den wichtigsten Nachfassern schicken.
```

Falls Sie die Cron-Datei lieber direkt bearbeiten, verwenden Sie diesen JSON-Block als Vorlage in `~/.openclaw/cron/jobs.json`:

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
    "message": "Nutze den lead-tracker-Skill und fuehre einen Review durch."
  },
  "delivery": {
    "mode": "announce",
    "to": "IHRE_TELEGRAM_CHAT_ID"
  }
}
```

## Dateien zum Nachsehen

- [`../skills/lead-tracker/SKILL.md`](../skills/lead-tracker/SKILL.md) - Hauptregeln des Skills
- [`../skills/lead-tracker/references/lead-schema.md`](../skills/lead-tracker/references/lead-schema.md) - Spalten der Lead-Liste
- [`../skills/lead-tracker/references/review-rules.md`](../skills/lead-tracker/references/review-rules.md) - Regeln für überfällige Leads
- [`../skills/lead-tracker/data/leads.csv`](../skills/lead-tracker/data/leads.csv) - lokale Lead-Liste
