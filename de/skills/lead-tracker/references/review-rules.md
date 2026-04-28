# Review-Regeln

Beschreibt, welche Leads beim Review-Lauf (wöchentlicher Cron oder manuelle Anfrage) gemeldet werden und wie die Ausgabe aussieht.

## Filter-Logik

Ein Lead ist **reviewbedürftig**, wenn **eine** der folgenden Bedingungen zutrifft:

### Bedingung 1: Offener Lead, Frist überschritten

- `status = offen`
- UND `letzter_kontakt` liegt länger zurück als die für den Typ geltende Frist (siehe `lead-types.md`)
- UND (falls Spalte `naechste_pruefung` existiert): `naechste_pruefung` ist leer ODER liegt in der Vergangenheit

### Bedingung 2: Wartender Lead, zu lange still

- `status = wartend`
- UND `letzter_kontakt` liegt mehr als 30 Tage zurück
- UND (falls Spalte `naechste_pruefung` existiert): `naechste_pruefung` ist leer ODER liegt in der Vergangenheit

### Bedingung 3: Explizit fällige Prüfung

- `status = offen` ODER `status = wartend`
- UND `naechste_pruefung` ist gesetzt und liegt am heutigen Tag oder in der Vergangenheit

## Sortierung

Älteste `letzter_kontakt` zuerst (älteste = dringendste).

## Ausgabeformat

### Kopfzeile

`Lead-Review [Datum der Ausführung]: [N] Leads brauchen Aufmerksamkeit.`

Wenn `N = 0`: Nur ausgeben `Lead-Review [Datum]: alles im grünen Bereich.` und keine weiteren Details.

### Pro Lead

```
▸ [Name], [Firma]
  Letzter Kontakt vor [X] Tagen. Nächster Schritt: [naechster_schritt].
  Notiz: [notiz — auf 80 Zeichen gekürzt]
```

### Abschlusszeile

`Antworte mit "erinnere mich morgen an [Name]" für einen Vormerker oder "vertage [Name] um 7 Tage" um die nächste Prüfung zu verschieben.`

## Fehlerbehandlung

- Wenn `./data/leads.csv` fehlt oder leer ist: Melde "Keine Lead-Liste gefunden — lege erst Leads an, bevor ich einen Review machen kann."
- Wenn ein Datumsfeld kein gültiges Format hat: Überspringe die Zeile nicht, sondern melde "Achtung: [Name, Firma] hat ein ungültiges Datum in letzter_kontakt — bitte prüfen."
