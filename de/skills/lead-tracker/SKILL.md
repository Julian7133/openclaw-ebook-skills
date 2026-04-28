---
name: lead-tracker
description: Verwaltet eine persönliche Lead-Liste für Vertriebs-Follow-Ups. Wird aktiv, wenn der Nutzer über Interessenten, Verkaufschancen, Kundenanfragen oder Follow-Ups spricht — zum Beispiel "trage einen neuen Lead ein", "welche Leads sind überfällig?", "wer hat mir seit Monaten nichts mehr geschrieben?", "scan meine Mails nach neuen Lead-Kandidaten". Arbeitet mit der Datei data/leads.csv innerhalb dieses Skill-Ordners.
---

# Lead-Tracker

Hilft dem Nutzer, offene Verkaufschancen nicht zu verlieren. Führt eine einfache CSV-basierte Lead-Liste und generiert proaktive Follow-Up-Erinnerungen.

## Datenablage

Die Lead-Liste liegt unter `./data/leads.csv` — relativ zu dieser SKILL.md. Trennzeichen: Semikolon. Kodierung: UTF-8. Zeilenende: Unix (LF).

Wenn die Datei beim ersten Aufruf nicht existiert oder nur den Header enthält: Die Datei ist bereits mit dem korrekten Header ausgestattet; du darfst direkt Zeilen anhängen. Falls die Datei fehlt, lies `references/lead-schema.md` und lege sie mit genau dem dort beschriebenen Header an.

## Standard-Aktionen

### Neuen Lead eintragen

Wenn der Nutzer einen Lead erwähnt — explizit ("trag folgenden Lead ein") oder im Gesprächsfluss ("ich war heute bei der Firma X, die brauchen ein Angebot"):

1. Lade `references/lead-schema.md` für die aktuelle Spaltendefinition, falls noch nicht geladen.
2. Extrahiere alle Informationen, die der Nutzer bereits genannt hat.
3. Frage gezielt nach Pflichtfeldern, die noch fehlen. Stelle **nur eine Frage pro Nachricht** — nicht fünf auf einmal.
4. Bei unklarem Status: frage "offen, wartend oder schon abgeschlossen?".
5. Hänge die Zeile an `./data/leads.csv` an (Trennzeichen Semikolon beachten, bei Notizen mit Semikolon diese in Anführungszeichen setzen).
6. Bestätige knapp: "Eingetragen: [Name, Firma]. Nächster Schritt: [was]."

Pflichtfelder: `name`, `firma`, `status`. Alle anderen können zunächst leer bleiben, sollen aber — wenn der Kontext sie nahelegt — direkt gefüllt werden.

### Bestehende Liste abfragen

- "Wie viele Leads habe ich?" → Anzahl, nach Status gruppiert.
- "Zeig mir die offenen Leads" → alle Zeilen mit `status=offen`, knapp formatiert.
- "Was weiß ich über [Firma]?" → alle Zeilen, deren `firma` oder `notiz` den Suchbegriff enthält.
- "Wann hatte ich zuletzt Kontakt mit [Name]?" → `letzter_kontakt` plus `notiz` der passenden Zeile.

### Überfällige Leads identifizieren

Für "welche Leads sind überfällig?" oder beim wöchentlichen Review-Cron:

1. Lade `references/lead-types.md` und `references/review-rules.md`.
2. Wende die dort definierten Fristen und Filter an.
3. Sortiere nach Dringlichkeit (älteste `letzter_kontakt` zuerst).
4. Formatiere nach dem in `review-rules.md` definierten Ausgabe-Format.

### Leads aus vorhandenen E-Mails erfassen

Wenn der Nutzer bittet, Mails nach Lead-Kandidaten zu durchsuchen (z.B. "scan meine Mails der letzten 30 Tage"):

1. Nutze den `himalaya`-Befehl (E-Mail-Skill aus Kapitel 11) für den gewünschten Zeitraum.
2. Sammle alle eingehenden Mails, extrahiere Absender-Adresse und -Name, Betreff, Datum.
3. Filtere aggressiv aus:
   - Keine `noreply@…`, `no-reply@…`, `newsletter@…`, `mailer-daemon@…`.
   - Keine Absender mit Freemail-Domains, die offensichtlich Privatpersonen sind (außer der Nutzer bittet explizit darum).
   - Keine Absender, deren E-Mail-Adresse bereits in `./data/leads.csv` steht.
4. Präsentiere die verbleibenden Kandidaten **einzeln**, nicht als Sammelliste. Pro Kandidat: Name (falls erkennbar), Firma (aus Domain abgeleitet oder Signatur), Betreff der letzten Mail, Datum.
5. Frage bei jedem: "Als Lead aufnehmen? (ja / nein / überspringen)"
6. Trage nur bestätigte Kandidaten ein. Nutze `naechster_schritt = "Erstkontakt klären"` als Default, sofern der Nutzer nichts anderes sagt.

## Anpassungen durch den Nutzer

Wenn der Nutzer das Verhalten des Skills ändern möchte, bearbeite die passende References-Datei direkt und bestätige dem Nutzer, was geändert wurde.

| Wunsch des Nutzers | Zu bearbeitende Datei |
|--------------------|----------------------|
| Neue Lead-Typen oder andere Fristen | `references/lead-types.md` |
| Andere Review-Logik oder Ausgabeformat | `references/review-rules.md` |
| Neue Spalten oder anderes Schema | `references/lead-schema.md` UND `./data/leads.csv` |

Bei Schema-Änderungen: Header-Zeile UND alle Datenzeilen konsistent anpassen. Alte Spalten, die entfernt werden sollen, vorher beim Nutzer bestätigen lassen.

## Kritische Regeln

- Verändere `./data/leads.csv` nur bei expliziter oder klar impliziter Bestätigung des Nutzers.
- Bei Unklarheit: nachfragen statt raten.
- Datumsformat immer `YYYY-MM-DD`.
- UTF-8-Kodierung beibehalten.
- Bei bulk-Änderungen (mehr als 3 Zeilen) vorher Zusammenfassung zeigen, dann Bestätigung abwarten.
