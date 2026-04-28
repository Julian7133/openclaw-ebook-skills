# Meeting-Prep-Regeln

Beschreibt, wie der Prep-Block für externe Termine aufgebaut wird. Nur externe Termine (Ort-Feld nicht leer) bekommen einen Prep-Block — interne Meetings werden im Briefing ohne Zusatzinformation gelistet.

## Grundidee

Der Prep soll den Nutzer in die Lage versetzen, in das Gespräch zu gehen, ohne vorher noch E-Mail-Verläufe oder den Lead-Tracker durchzublättern. Er beantwortet drei Fragen:

1. **Was war der letzte Kontakt?** — Datum + kurze Sachstandsbeschreibung
2. **Was sind offene Punkte?** — Konkret, was noch nicht geklärt ist
3. **Gibt es Lead-Kontext?** — Status aus `lead-tracker`, falls die Firma dort vorkommt

Was der Prep **nicht** tut: Er fasst nicht eigene Angebote zusammen, er rezitiert nicht Mail-Inhalte wörtlich, er produziert keine Gesprächsstrategie. Er beschreibt den Sachstand so, wie ein guter Assistent ihn einem Chef auf einem Merkzettel zusammenfassen würde.

## Format des Prep-Blocks

Nach der Termin-Zeile eingerückt, kursiv angedeutet durch schmalen Textblock (keine explizite Formatierung — reine Prosa mit Umbrüchen):

```
    Letzter Austausch: [1 Zeile Sachstand mit Datum]
    Offene Punkte: [1 Zeile, das Wichtigste]
    [Optional: Lead-Status, falls passend]
```

Maximal drei Zeilen pro Termin. Wenn nichts davon recherchierbar ist: stattdessen eine Zeile:
```
    Kein Vorkontext gefunden — wahrscheinlich Erstgespräch.
```

## Datenquellen für den Prep

### 1. Mail-Verlauf

Durchsuche `himalaya` nach Nachrichten, deren Absender-Domain mit dem Firmennamen oder der Termin-Notiz zusammenhängt. Extrahiere:
- Letzter eingehender Austausch: Datum, Betreff, 1-Satz-Inhalt
- Letzter ausgehender Austausch: Datum, Betreff, 1-Satz-Inhalt
- Daraus die relevantere Zeile für "Letzter Austausch" ableiten

Bei mehrdeutigen Funden (z.B. mehrere Personen derselben Firma): den zur Termin-Beschreibung passenden Austausch wählen.

### 2. Lead-Tracker

Lade `~/.openclaw/workspace/skills/lead-tracker/data/leads.csv`. Suche nach passender `firma`. Wenn gefunden:
- `naechster_schritt` in die Zeile "Offene Punkte" aufnehmen, wenn nichts Besseres aus dem Mail-Verlauf kommt.
- `notiz` als zusätzlichen Kontext erwägen, aber nur wenn nicht redundant.
- Falls der Lead überfällig ist: explizit vermerken (siehe unten).

### 3. Termin-Beschreibung

Der Text im Kalender-Eintrag selbst (das `description`-Feld) hat höchste Priorität. Wenn der Nutzer dort bereits Notizen hinterlegt hat, übernimm diese 1:1 als "Offene Punkte" und suche Mail/Lead-Daten nur ergänzend.

## Priorisierung bei Konflikten

Wenn aus verschiedenen Quellen unterschiedliche Sachstände herauskommen:

1. Neueste Information gewinnt.
2. Bei gleichem Datum: Termin-Beschreibung > Mail-Verlauf > Lead-Tracker.
3. Bei Widersprüchen: beide Stände knapp erwähnen ("Laut E-Mail vom 19.03. Angebot versendet, Lead-Status zeigt noch 'offen'").

## Überfällige Leads im Prep

Wenn die Firma im Lead-Tracker als überfällig gilt (nach den Regeln in `lead-tracker/references/review-rules.md`): Ein zusätzlicher, eigener Vermerk als dritte Prep-Zeile:

```
    ⚠️ Lead seit [X] Tagen ohne Kontakt — gute Gelegenheit zum Nachfassen.
```

Das Warnzeichen nur hier — nicht im normalen Termin-Text.

## Performance

Dieser Prep wird morgens für typischerweise 1–3 externe Termine gebaut. Pro Termin entstehen 1–2 Mail-Suchen und 1 CSV-Lesung. Bei >5 externen Terminen an einem Tag auf knappere Prep-Blöcke (nur "Letzter Austausch") reduzieren, sonst wird das Briefing zu lang.

## Beispiele

### Vollständig recherchierbarer Termin

```
• 10:30 Besuch Schumann Präzisionswerk (Augsburg)
    Letzter Austausch: Anfrage "Serienteile Q3" vom 14.03.,
    Angebot am 19.03. versendet.
    Offene Punkte: Preisstaffel ab 5.000 Stück nicht finalisiert.
```

### Mit überfälligem Lead

```
• 14:00 Call Fischer-Werke KG (Telefon)
    Letzter Austausch: Angebot Solaranlage am 15.02. versendet.
    Offene Punkte: Q2-Entscheidung steht aus.
    ⚠️ Lead seit 71 Tagen ohne Kontakt — gute Gelegenheit zum Nachfassen.
```

### Ohne Vorkontext

```
• 16:00 Besuch Novaflex Beschichtung (München)
    Kein Vorkontext gefunden — wahrscheinlich Erstgespräch.
```
