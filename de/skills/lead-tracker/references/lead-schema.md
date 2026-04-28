# Lead-Schema

Definiert die Spaltenstruktur der Lead-Liste `./data/leads.csv`.

## Aktueller Header

```
name;firma;email;letzter_kontakt;naechster_schritt;status;notiz
```

## Spaltendefinitionen

| Spalte | Typ | Pflicht | Beschreibung |
|--------|-----|---------|--------------|
| `name` | Text | ja | Ansprechpartner (Vor- und Nachname) |
| `firma` | Text | ja | Unternehmen |
| `email` | Text | nein (stark empfohlen) | E-Mail-Adresse; wird für Dedupe bei Mail-Scans genutzt |
| `letzter_kontakt` | Datum | nein | Format `YYYY-MM-DD`; leer = noch nie kontaktiert |
| `naechster_schritt` | Text | nein | Freitext: was als Nächstes getan werden sollte |
| `status` | Enum | ja | Einer von: `offen`, `wartend`, `gewonnen`, `verloren` |
| `notiz` | Text | nein | Freitext: Gesprächsinhalt, Kontext, Vorbehalte |

## Format-Regeln

- **Trennzeichen:** Semikolon (`;`)
- **Kodierung:** UTF-8
- **Zeilenende:** Unix (LF)
- **Felder mit Semikolon:** in doppelte Anführungszeichen setzen
- **Felder mit doppelten Anführungszeichen:** diese verdoppeln (`""`)
- **Leere Felder:** einfach leer lassen, keine Füllwerte wie `NULL` oder `-`

## Erweiterungen

Wenn der Nutzer das Schema erweitern möchte (z.B. eine Spalte `typ` oder `naechste_pruefung`), aktualisiere diese Datei:

1. Trage die neue Spalte in die Tabelle ein.
2. Beschreibe Typ, Pflicht-Status, Werte.
3. Aktualisiere den Header in diesem Abschnitt.
4. Passe `./data/leads.csv` an: Header und alle bestehenden Zeilen.
