# Kapitel 15: Workflow 5 — Das Markt-Radar

## Brave API-Key eintragen

```bash
nano ~/.openclaw/.env
```

```env
BRAVE_API_KEY=IhrTokenHier
```

```bash
chmod 600 ~/.openclaw/.env
openclaw gateway restart
```

## Websuche testen

```text
Kannst du bitte im Web suchen, was VDMA diese Woche zum Thema Lieferkette veröffentlicht hat?
```

## Skill installieren

```bash
cd ~/.openclaw/workspace/begleit-repo
git pull
mkdir -p ~/.openclaw/workspace/skills
cp -r de/skills/market-radar ~/.openclaw/workspace/skills/
openclaw gateway restart
```

## Quellen aufsetzen

```text
Lass uns meine Radar-Quellen aufsetzen. Direkte Wettbewerber sind: Sittner Präzisionstechnik in Günzburg, Gebrüder Meier Metallverarbeitung, Stahlwerk Augsburg GmbH, Novaflex Beschichtung, Rapidform aus dem Allgäu. An Branchenmedien will ich maschinenmarkt.vogel.de, industrie.de und den VDMA unter vdma.org beobachten. Regulatorik lassen erstmal weg.
```

## Manuell testen

```text
Führe meinen Markt-Radar einmal manuell aus.
```

```text
Radar bitte.
```

## Report anzeigen

```text
Zeig mir den vollen Radar-Report von heute.
```

## Cron bearbeiten

```bash
nano ~/.openclaw/cron/jobs.json
```

## Wochenreport

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
    "message": "Führe den market-radar-Skill aus und erstelle den Wochenreport."
  },
  "delivery": {
    "mode": "announce",
    "to": "IHRE_TELEGRAM_CHAT_ID"
  }
}
```

## Quelle ersetzen

```text
Rapidform hat auf der Website nichts mehr. Ersetze den Eintrag durch die LinkedIn-Firmenseite.
```

## Keywords ergänzen

```text
Nimm „Nearshoring" und „Rückverlagerung" als Keywords in topics.md auf. Beide sind für uns relevant.
```

## Suchzeit anpassen

```text
Ändere die Default-Suchzeit auf 14 Tage zurück statt 10, damit bei Feiertags-Wochen nichts durchrutscht.
```
