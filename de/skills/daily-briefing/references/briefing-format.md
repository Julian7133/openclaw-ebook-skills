# Briefing-Format

Definiert das Layout der Morgen-Nachricht. Die Reihenfolge der Abschnitte entspricht der gewünschten Lese-Priorität: Was der Nutzer zuerst wissen muss, steht oben.

## Gesamt-Struktur

```
☀️ Morgen-Briefing — [Wochentag], [Datum]

[Termine-Abschnitt]

[E-Mail-Abschnitt]

[Leads-Abschnitt — nur montags]

[Aktionen-Abschnitt]
```

Gesamtlänge: kompakt, Ziel ist unter einer Minute Lesezeit. Wenn ein Abschnitt leer ist, wird er mit einer knappen Negativmeldung ersetzt, nicht weggelassen (siehe unten).

## Termine-Abschnitt

### Format

```
📅 Heute: [N] Termine.

[Für jeden Termin:]
• [HH:MM] [Titel] [(Ort)]
[Bei externen Terminen: Prep-Block, siehe meeting-prep-rules.md]
```

### Leere Variante

Wenn keine Termine anstehen:
```
📅 Heute: keine Termine.
```

### Abgrenzung intern vs. extern

- **Intern** (kein Ort angegeben): Nur Zeile mit Uhrzeit und Titel. Kein Prep.
- **Extern** (Ort angegeben): Uhrzeit, Titel, Ort in Klammern, plus Prep-Block nach den Regeln in `meeting-prep-rules.md`.

## E-Mail-Abschnitt

### Format

```
📧 Nacht-E-Mails: [N] relevant.

[Pro Mail, max 3:]
• [Absender]: [Betreff — auf 60 Zeichen gekürzt]
```

Wenn mehr als 3 relevante Mails vorliegen:
```
... sowie [N-3] weitere.
```

### Leere Variante

```
📧 Nacht-E-Mails: nichts Dringendes.
```

### Filter-Kriterien

Identisch zur Heartbeat-Regel aus `~/.openclaw/workspace/HEARTBEAT.md`:
- Nur ungelesene Nachrichten aus dem Zeitraum seit Mitternacht.
- Keine Newsletter, Noreply-Absender, Systemmails.
- Bei wenn-dann-Regeln aus der Heartbeat-Datei (z.B. „Priorität HOCH bei @automobil-ag.com") hier nur die HOCH-Kandidaten aufnehmen.

## Leads-Abschnitt

### Wann erscheint dieser Abschnitt

Nur montags. An anderen Werktagen wird der gesamte Abschnitt ausgelassen — er erscheint nicht einmal leer, sondern gar nicht.

### Format

```
🎯 Leads, die dran sind:

[Pro Lead, max 3:]
• [Name], [Firma] — seit [X] Tagen still. [Nächster Schritt]
```

### Leere Variante (auch nur montags)

```
🎯 Leads: alles im grünen Bereich.
```

### Datenherkunft

Die Auswahl übernimmt der `lead-tracker`-Skill. Lade dessen `references/review-rules.md` und wende die Filter-Logik an. Für das Briefing reichen die ersten drei Leads der Dringlichkeitssortierung — der wöchentliche Lead-Review-Cron (Kapitel 13) liefert die vollständige Liste separat.

## Aktionen-Abschnitt

### Format

```
💬 Optionen:
• [Aktion 1]
• [Aktion 2]
• [Aktion 3, falls sinnvoll]
```

Maximal drei Aktionen. Die Aktionen werden dynamisch aus dem Tagesinhalt abgeleitet:

### Regeln für die Aktions-Auswahl

| Tagesinhalt | Vorgeschlagene Aktion |
|-------------|----------------------|
| Externer Termin heute mit überfälligem Lead-Status | "Bereite [Termin-Kontakt] vor" |
| E-Mail von bekanntem Key-Account in der Nacht | "Fasse die [Absender]-Mail zusammen und schlag eine Antwort vor" |
| Montag + überfällige Leads vorhanden | "Schreib einen Follow-Up-Entwurf an [Name 1]" |
| Termin-Konflikt entdeckt | "Löse Konflikt zwischen [Termin 1] und [Termin 2]" |
| Nichts Dringendes | Abschnitt komplett weglassen |

Wenn mehrere Regeln zutreffen: priorisiere Aktionen, die zu **externen Terminen heute** führen (die haben Zeitdruck), vor generischen Mail- oder Lead-Aktionen.

## Beispiel (Thomas Weber, Montag)

```
☀️ Morgen-Briefing — Montag, 27.04.2026

📅 Heute: 3 Termine.
• 10:30 Besuch Schumann Präzisionswerk (Augsburg)
    Letzter Austausch: Anfrage "Serienteile Q3" vom 14.03.,
    Angebot Ihrerseits am 19.03. versendet. Seither kein Rückmelden.
    Offene Punkte: Preisstaffel ab 5.000 Stück nicht finalisiert.
• 13:00 Produktions-Jour-Fixe
• 15:00 Call mit Herrn Vogel (Vogel Automation)

📧 Nacht-E-Mails: 2 relevant.
• Automobil AG / Reklamation: Verspätete Lieferung Charge 8412 —
  bitte Rückmeldung bis heute 12 Uhr
• Schumann Präzisionswerk: Bestätigt 10:30, bringt Einkäufer mit

🎯 Leads, die dran sind:
• Hans Fischer, Fischer-Werke KG — seit 71 Tagen still.
  Nachfassen nach Q2.
• Martina Meier, Meier Formenbau — seit 22 Tagen still.
  Rückruf wegen Werkzeugauftrag.

💬 Optionen:
• Bereite Schumann-Gespräch vor (Letzter Stand zusammenfassen)
• Entwurf für Reklamations-Antwort an Automobil AG
• Follow-Up-Mail an Hans Fischer
```

## Gestaltungsprinzipien

- **Keine Einleitung, keine Abschlussfloskel.** Das Briefing beginnt mit der Überschrift und endet nach den Optionen.
- **Emojis als Abschnittsmarker**, nicht dekorativ. Ein Emoji pro Abschnitt: ☀️📅📧🎯💬.
- **Zahlen in der ersten Zeile jedes Abschnitts.** Der Leser soll auf einen Blick erfassen, ob Aufmerksamkeit nötig ist (3 Termine, 0 Mails, etc.).
- **Datum auf Deutsch**, Wochentag zuerst. Format `DD.MM.YYYY`.
