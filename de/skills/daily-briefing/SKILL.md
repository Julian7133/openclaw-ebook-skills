---
name: daily-briefing
description: Erstellt ein kompaktes Morgen-Briefing für den Geschäftstag. Wird typischerweise per Cron um 07:00 Uhr im Main-Kontext ausgelöst (systemEvent), kann aber auch manuell per Chat angestoßen werden ("gib mir mein Briefing"). Bündelt Informationen aus Kalender, E-Mail und Lead-Tracker zu einem strukturierten Tagesüberblick und bietet Aktionen zum Nachhaken an.
---

# Daily Briefing

Liefert morgens einen strukturierten Überblick über den Geschäftstag. Anders als die laufenden Heartbeat-Workflows (E-Mail, Kalender) geht es hier nicht um Einzelmeldungen, sondern um die *Sichtkarte des Tages* — eine kompakte Nachricht, die in unter einer Minute gelesen werden kann und konkrete Aktionen anbietet.

## Ausführungskontext

Dieser Skill läuft im Main-Session-Kontext. Das ist bewusst gewählt:

- Der Nutzer soll auf das Briefing direkt per Chat antworten können ("Bereite bitte das Schumann-Gespräch vor").
- Der Briefing-Kontext soll in der laufenden Unterhaltung verfügbar bleiben, damit Folgefragen wie "Was steht nochmal bei Schumann an?" ohne erneutes Laden beantwortet werden können.

Aus demselben Grund läuft der zugehörige Cron-Job mit `sessionTarget: "main"` und `payload.kind: "systemEvent"`, nicht in isolierter Session.

## Ablauf

Wenn das Briefing angefordert wird (durch den Cron-systemEvent oder manuelle Anfrage):

1. Lade `references/briefing-format.md` für das Ausgabeformat.
2. Lade `references/meeting-prep-rules.md` für die Prep-Logik bei externen Terminen.
3. Sammle die Datenquellen (siehe unten).
4. Formatiere nach `briefing-format.md`.
5. Poste die Nachricht im Main-Thread an den konfigurierten Channel.

### Datenquellen

**Termine heute**
Nutze `khal list today` (Kalender aus Kapitel 12). Extrahiere: Start-Zeit, Titel, Ort, Beschreibung. Für alle Termine mit einer nicht-leeren Ort-Angabe gilt: externer Termin, Meeting-Prep nach den Regeln aus `meeting-prep-rules.md`.

**Wichtigste E-Mails aus der Nacht**
Nutze `himalaya envelopes list --folder INBOX` (E-Mail aus Kapitel 11), gefiltert auf die Zeit seit Mitternacht. Filtere aggressiv nach den Regeln der Heartbeat-Vorgabe aus `~/.openclaw/workspace/HEARTBEAT.md` (Newsletter/Systemmails raus). Maximal drei Mails im Briefing — wenn mehr, einen Sammler "sowie N weitere" ergänzen.

**Überfällige Leads**
Nur montags (Wochentag = Montag). Nutze den `lead-tracker`-Skill, rufe intern dessen Review-Regeln auf. Übernimm die ersten drei Zeilen des Review-Ergebnisses. Wenn heute kein Montag, diesen Abschnitt weglassen.

### Aktionen anbieten

Am Ende des Briefings werden Aktionen angeboten, auf die der Nutzer per Chat antworten kann. Die Liste der Optionen wird dynamisch erzeugt je nach Tagesinhalt — siehe `references/briefing-format.md` Abschnitt „Aktionen".

## Ruhetage

- Samstag und Sonntag: Kein Briefing (Cron-Expression `0 7 * * 1-5` steuert das bereits; der Skill selbst prüft nicht).
- Feiertage (optional, nicht implementiert): Kann der Nutzer später als Liste in einer neuen `references/feiertage.md` ergänzen. Für den ersten Release nicht nötig.

## Fehlertoleranz

Wenn eine Datenquelle nicht verfügbar ist (z.B. `himalaya`-Aufruf schlägt fehl, weil der Mail-Server kurz nicht erreichbar ist):

- **Niemals** das gesamte Briefing abbrechen.
- Den betroffenen Abschnitt durch eine klare Notiz ersetzen: "E-Mails konnten nicht abgerufen werden — Mail-Server nicht erreichbar."
- Alle anderen Abschnitte normal ausgeben.

Ein unvollständiges Briefing ist immer besser als gar keins.

## Anpassungen durch den Nutzer

| Wunsch des Nutzers | Zu bearbeitende Datei |
|--------------------|----------------------|
| Anderes Layout, andere Abschnitte, andere Reihenfolge | `references/briefing-format.md` |
| Andere Kriterien, wann ein Meeting als "Prep-bedürftig" gilt | `references/meeting-prep-rules.md` |
| Weniger/mehr Mails, andere Filter, andere Max-Anzahl | `references/briefing-format.md` |
| Briefing auch am Wochenende | Cron-Expression ändern (außerhalb des Skills) |

Bei Anpassungen: Datei direkt bearbeiten und dem Nutzer knapp bestätigen, was geändert wurde.
