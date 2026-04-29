# Kapitel 10: HEARTBEAT.md & Cron

## HEARTBEAT.md erstellen

```bash
nano ~/.openclaw/workspace/HEARTBEAT.md
```

## HEARTBEAT.md Template (kopieren und anpassen)

```markdown
# Heartbeat Dispatcher

## Zeitplan
- **Intervall:** Alle 30 Minuten
- **Aktive Zeit:** Mo-Fr, 08:00–20:00 Uhr

**Ruhezeiten:**
- Wochenenden: HEARTBEAT_OK sofort
- Nach 20:00 Uhr: HEARTBEAT_OK sofort
- Vor 08:00 Uhr: HEARTBEAT_OK sofort

## Aufgaben beim Aufwachen (in Prioritätsreihenfolge)

1. **[E-Mail-Check]** Alle 3 Stunden: Posteingang auf dringende
   Nachrichten prüfen. Nur Nachrichten der letzten 3 Stunden.
   Ignoriere Newsletter und Marketing.

2. **[Kalender-Check]** Alle 6 Stunden: Nächste Termine prüfen.
   Nur melden wenn: Start in <4h, neuer Termin seit letztem Check,
   oder Termin-Konflikt.

3. **[Erinnerungen]** Bei Fälligkeit: Offene Aufgaben, Deadlines,
   Angebots-Fristen.

## Ruhezeiten-Regeln
- Samstag/Sonntag: Keine aktiven Checks, HEARTBEAT_OK.
- Nach 18:00 Uhr: Keine E-Mail-Checks mehr (Kalender und andere
  Checks laufen bis 20:00 weiter).
- Nach 20:00 Uhr: Alles still, nur HEARTBEAT_OK.

## Antwort-Regeln
Wenn ein Check etwas findet:
- Kurz und präzise, maximal 3 Sätze.
- Konkrete Handlung, wenn eine erforderlich ist.
- HEARTBEAT_OK, wenn nichts zu tun ist.
```

**Anpassungen:** Zeiten und Aufgaben an deine Situation anpassen.

Speichern: `Strg+O` → Enter → `Strg+X`

## Cron Jobs erstellen (optional, für fortgeschrittene Automatisierung)

```bash
# Morgen-Briefing täglich 08:30
openclaw cron add --name "morning-briefing" --schedule "30 8 * * *" --agent "morning-briefing"

# Beispiel für Job-Suche
openclaw cron add --name "job-search" --schedule "5 3 * * *" --agent "job-search-runner"
```

## Cron Jobs verwalten

```bash
# Liste aller Jobs
openclaw cron list

# Job aktivieren/deaktivieren
openclaw cron enable JOB_ID
openclaw cron disable JOB_ID

# Job löschen
openclaw cron delete JOB_ID
```

## Gateway neu starten (nach HEARTBEAT.md Änderungen)

```bash
openclaw gateway restart
```

---

**Erklärungen & Anpassungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides