# Lead-Typen und Follow-Up-Fristen

Definiert, nach wie vielen Tagen ohne Kontakt ein Lead als überfällig gilt. Der Nutzer kann Typen jederzeit ergänzen oder Fristen anpassen — editiere in diesem Fall direkt diese Datei.

## Standardtyp

Wenn die Spalte `typ` in `./data/leads.csv` nicht existiert oder leer ist, gilt für alle Leads:

- **Frist:** 14 Tage ohne Kontakt
- **Gilt für:** Leads mit `status = offen`

## Lead-Typen (falls Spalte `typ` vorhanden)

Wenn der Nutzer typabhängige Fristen eingeführt hat und die CSV eine Spalte `typ` hat, gelten folgende Fristen:

| Typ | Frist (Tage) | Typischer Anwendungsfall |
|-----|--------------|--------------------------|
| `schnell` | 3 | Schnelle Verkaufszyklen, Austausch-Teile, transaktionale Anfragen |
| `normal` | 14 | Standard-Verkaufszyklus |
| `langsam` | 60 | Große Projekte, lange Entscheidungswege, strategische Accounts |

## Fallback-Regel

Wenn ein Lead einen Typ hat, der in obiger Tabelle nicht definiert ist: verwende die Frist des Typs `normal` (14 Tage) und weise den Nutzer einmalig darauf hin, dass der Typ unbekannt ist.

## Wartende Leads

Unabhängig vom Typ: Ein Lead mit `status = wartend` wird erst nach **30 Tagen** ohne Kontakt gemeldet, es sei denn, er hat eine explizite `naechste_pruefung` (siehe `review-rules.md`).
