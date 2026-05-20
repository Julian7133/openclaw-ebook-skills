# Kapitel 10: HEARTBEAT.md & Cron

Copy-Paste-Blatt für proaktive Checks, optionalen Cron und den ersten End-to-End-Test.

## HEARTBEAT.md erstellen

```bash
nano ~/.openclaw/workspace/HEARTBEAT.md
```

## Vorlage

```markdown
# Heartbeat Dispatcher

Standard-Prozedur: Lese HEARTBEAT.md → Führe überfällige Checks durch → REPLY HEARTBEAT_OK.

## Zeitplan

- **Intervall:** Alle 30 Minuten
- **Aktive Zeit:** Mo–Fr, 08:00–20:00 Uhr

**Ruhezeiten:**

- Wochenenden: HEARTBEAT_OK sofort
- Nach 20:00 Uhr: HEARTBEAT_OK sofort
- Vor 08:00 Uhr: HEARTBEAT_OK sofort

## Aufgaben beim Aufwachen (in Prioritätsreihenfolge)

1. **[E-Mail-Check]** Alle 3 Stunden: Posteingang auf dringende Nachrichten prüfen. Nur Nachrichten der letzten 3 Stunden. Ignoriere Newsletter und Marketing.
2. **[Kalender-Check]** Alle 6 Stunden: Nächste Termine prüfen. Nur melden wenn: Start in <4h, neuer Termin seit letztem Check, oder Termin-Konflikt.
3. **[Erinnerungen]** Bei Fälligkeit: Offene Aufgaben, Deadlines, Angebots-Fristen.

## Ruhezeiten-Regeln

- Samstag/Sonntag: Keine aktiven Checks, HEARTBEAT_OK.
- Nach 18:00 Uhr: Keine E-Mail-Checks mehr. Kalender und andere Checks laufen bis 20:00 weiter.
- Nach 20:00 Uhr: Alles still, nur HEARTBEAT_OK.

## Antwort-Regeln

Wenn ein Check etwas findet:

- Kurz und präzise, maximal 3 Sätze.
- Konkrete Handlung, wenn eine erforderlich ist.
- HEARTBEAT_OK, wenn nichts zu tun ist.
```

Speichern: `Strg+O` → Enter → `Strg+X`

## Beispiel-Anpassung für Beratung

```markdown
## Aufgaben beim Aufwachen (in Prioritätsreihenfolge)

1. **[E-Mail-Check]** Alle 3 Stunden: Nachrichten von bekannten Kunden und Interessenten prüfen. Nur Absender aus bekannten Kontakten. Newsletter und Bewerbungsportale ignorieren.
2. **[Kalender-Check]** Alle 2 Stunden: Nächste Termine prüfen. 15 Minuten vor jedem Termin erinnern. Nur melden wenn: Start in <2h, neuer Termin seit letztem Check, oder Konflikt.
3. **[Server-Check]** Täglich um 07:00: Disk-Auslastung und RAM des Demo-Servers prüfen. Alert wenn Disk >80% oder RAM >90%.
```

## Optionaler Cron: Morning Briefing

Der Cron-Abschnitt ist optional. Das Manuskript empfiehlt, ihn später mit OpenClaw oder Claude Code an die echte Umgebung anzupassen.

Skill-Datei:

```bash
nano ~/.openclaw/workspace/skills/morning-briefing/SKILL.md
```

```markdown
# Morning Briefing Skill

## Aufgabe

Erstelle jeden Morgen um 08:30 Uhr ein strukturiertes Briefing für den Nutzer. Sende das Ergebnis direkt auf Telegram.

## Briefing-Format

1. **Termine heute:** Liste der nächsten 3 Termine (Uhrzeit, Titel)
2. **E-Mails:** Top 5 wichtigste ungelesene E-Mails (Absender, Betreff)
3. **Offene Aufgaben:** Gibt es etwas Dringendes?
4. **Wetter:** Aktuelles Wetter in Augsburg (optional)

## Ausführung

- Sende das Briefing direkt per Telegram (message tool).
- Keine langen Analysen — nur Fakten.
- Wenn nichts Dringendes: „Keine Eilmeldungen. Schönen Tag noch."
```

Cron per CLI (Syntax je nach OpenClaw-Version mit `openclaw cron --help` prüfen):

```bash
openclaw cron add \
  --name "Morning Briefing" \
  --schedule "0 8 * * 1-5" \
  --agent "morning-briefing" \
  --timeout 300 \
  --tz "Europe/Berlin"
```

Alternativ als JSON in `~/.openclaw/cron/jobs.json`:

```json
{
  "agentId": "morning-briefing",
  "name": "Morning Briefing",
  "enabled": true,
  "schedule": {
    "kind": "cron",
    "expr": "0 8 * * 1-5",
    "tz": "Europe/Berlin"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Run morning briefing. Follow skills/morning-briefing/SKILL.md.",
    "timeoutSeconds": 300
  },
  "delivery": {
    "mode": "announce",
    "to": "YOUR_CHAT_ID"
  }
}
```

Cron-Jobs verwalten:

```bash
openclaw cron list
openclaw cron enable JOB_ID
openclaw cron disable JOB_ID
openclaw cron delete JOB_ID
```

Cron-Beispiele:

```text
0 8 * * 1-5    Jeden Mo–Fr um 08:00
30 9 * * *     Täglich um 09:30
0 18 * * 5     Jeden Freitag um 18:00
0 0 1 * *      Ersten Tag jedes Monats um Mitternacht
*/15 * * * *   Alle 15 Minuten
```

## Gateway neu starten

```bash
openclaw gateway restart
```

## Test-Lauf

Dem Assistenten über Telegram, Discord oder den eingerichteten Kanal schreiben:

```text
Hi
```

Wenn keine Antwort kommt:

```bash
openclaw gateway status
openclaw logs --tail 50
```

## Kapitel-Check

- `HEARTBEAT.md` existiert und enthält Zeiten, Aufgaben, Ruhezeiten und Antwortregeln
- Optionaler Cron wurde entweder bewusst ausgelassen oder mit aktueller CLI/JSON-Syntax eingerichtet
- `openclaw gateway status` ist unauffällig
- Testnachricht `Hi` wurde beantwortet

---

**Erklärungen & Anpassungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides
