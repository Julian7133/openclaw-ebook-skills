# Kapitel 15: Workflow 5 - Das Markt-Radar

Diese Seite ist der schnelle Einstieg zum Markt-Radar. Der Skill selbst liegt im Ordner [`../skills/market-radar/`](../skills/market-radar/). Hier finden Sie die Befehle, Prompts und den wöchentlichen Cron-Block aus dem Kapitel.

Der Markt-Radar sucht wöchentlich nach neuen Hinweisen bei Wettbewerbern und Branchenquellen, filtert nach Ihren Kernthemen und legt einen kurzen Report ab.

Die Firmennamen in den Beispieldateien sind fiktiv. Ersetzen Sie sie durch Ihre echten Wettbewerber und Quellen.

## 1. Brave Search API vorbereiten

Für die Websuche braucht OpenClaw einen Suchanbieter. Im Kapitel wird Brave Search verwendet.

Kopieren Sie diesen Prompt in OpenClaw, wenn Sie den API-Key noch nicht hinterlegt haben:

```text
Hilf mir, meinen Brave Search API-Key sicher für OpenClaw zu hinterlegen. Ich möchte ihn nicht in Klartext in einer Skill-Datei speichern. Erkläre mir kurz, wo der Key abgelegt wird, und teste danach mit einer einfachen Websuche, ob der Zugriff funktioniert.
```

Wenn Ihre OpenClaw-Installation Umgebungsvariablen über eine `.env`-Datei lädt, ist dies die übliche Zielvariable:

```bash
cd ~/.openclaw
printf '\nBRAVE_API_KEY=hier-ihren-key-eintragen\n' >> .env
chmod 600 .env
openclaw gateway restart
```

## 2. Begleit-Repo aktualisieren

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

## 3. Skill in OpenClaw kopieren

```bash
cd ~/.openclaw/workspace
mkdir -p skills
cp -r begleit-repo/de/skills/market-radar skills/
```

## 4. Datenordner prüfen

Diese Befehle stellen sicher, dass die Ablage für bereits gesehene URLs und Reports existiert:

```bash
cd ~/.openclaw/workspace/skills/market-radar
mkdir -p data/reports
touch data/seen-urls.txt data/reports/.gitkeep
```

## 5. Eigene Quellen eintragen

Öffnen Sie die Quellen-Datei mit OpenClaw oder bearbeiten Sie sie direkt:

```text
Bitte öffne den Market-Radar-Skill und passe `references/sources.md` an. Ersetze die fiktiven Beispiel-Wettbewerber durch diese echten Quellen: [hier Ihre Wettbewerber und Branchenquellen einfügen]. Behalte das Tabellenformat bei.
```

Danach passen Sie die Themen an:

```text
Bitte öffne `references/topics.md` im Market-Radar-Skill. Ersetze die Beispielthemen durch die Themen, die für mein Geschäft wichtig sind: [hier Ihre Themen einfügen]. Halte die Liste eher eng, damit der Report nicht zu allgemein wird.
```

## 6. Markt-Radar manuell testen

```text
Führe den Market-Radar einmal manuell aus. Nutze die Quellen aus `references/sources.md`, filtere nach `references/topics.md`, überspringe URLs aus `data/seen-urls.txt` und schreibe den Report nach `data/reports/market-radar-YYYY-MM-DD.md`. Zeige mir danach eine kurze Zusammenfassung.
```

Wenn die Suche wegen Rate-Limit oder fehlendem API-Key scheitert, soll OpenClaw den Lauf abbrechen und die Ursache klar nennen.

## 7. Wöchentlichen Cron eintragen

Verwenden Sie diesen JSON-Block als Vorlage in `~/.openclaw/cron/jobs.json`:

```json
{
  "agentId": "main",
  "name": "Markt-Radar",
  "enabled": true,
  "schedule": {
    "kind": "cron",
    "expr": "30 7 * * 5",
    "tz": "Europe/Berlin"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Fuehre den market-radar-Skill aus und erstelle den Wochenreport."
  },
  "delivery": {
    "mode": "announce",
    "to": "IHRE_TELEGRAM_CHAT_ID"
  }
}
```

Oder lassen Sie OpenClaw den Eintrag vorbereiten:

```text
Lege einen wöchentlichen Cron-Job für den Market-Radar an. Er soll freitags um 07:30 in einer isolierten Session laufen, den market-radar-Skill nutzen, einen Report in `data/reports/` speichern und mir nur eine kurze Zusammenfassung schicken. Bitte zeige mir den JSON-Block, bevor du ihn speicherst.
```

## 8. Gateway neu starten

```bash
openclaw gateway restart
```

## Dateien zum Nachsehen

- [`../skills/market-radar/SKILL.md`](../skills/market-radar/SKILL.md) - Hauptregeln des Skills
- [`../skills/market-radar/references/sources.md`](../skills/market-radar/references/sources.md) - Wettbewerber und Quellen
- [`../skills/market-radar/references/topics.md`](../skills/market-radar/references/topics.md) - Kernthemen
- [`../skills/market-radar/references/report-format.md`](../skills/market-radar/references/report-format.md) - Report-Aufbau
- [`../skills/market-radar/references/seen-urls-rules.md`](../skills/market-radar/references/seen-urls-rules.md) - Regeln gegen doppelte Funde
