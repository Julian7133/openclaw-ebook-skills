# Aktions-Regeln

Beschreibt, wie die "Optionen" am Ende des Briefings ausgewählt werden. Dieses Dokument ergänzt `briefing-format.md` (welches nur das Layout festlegt) um die Logik, welche Aktionen wann sinnvoll sind.

## Grundprinzip

Aktionen sind keine Pflicht. Wenn der Tag ruhig ist, bleibt der Optionen-Abschnitt weg. Die Qualität einer Option liegt darin, dass sie **eine konkrete Folge-Handlung** anstößt — nicht, dass sie eine Lücke füllt.

Eine gute Aktion hat drei Eigenschaften:
1. **Zeitnah relevant** — sie bezieht sich auf etwas, das heute oder in den nächsten zwei Tagen passiert
2. **Auf einen Klick umsetzbar** — der Nutzer antwortet mit einem kurzen „ja" / „mach" / „erstelle das" statt einer langen Anweisung
3. **Produktiv reibungsreduzierend** — sie erspart dem Nutzer eine Arbeit, die er sowieso machen müsste

## Aktionsquellen

Die folgenden Muster in der Tagesdatenlage rechtfertigen eine Aktion:

### Priorität 1: Externe Termine mit Prep-Lücke

**Auslöser:** Externer Termin heute, bei dem der Prep-Block unvollständig oder der Lead überfällig ist.

**Vorgeschlagene Aktion:**
```
Bereite [Firmenname]-Gespräch vor (letzten Stand zusammenfassen)
```

**Was das bedeutet, wenn der Nutzer zustimmt:** Der Assistent erstellt eine ausführlichere 5–10-Zeilen-Zusammenfassung des Kontakthistorien-Verlaufs mit der Firma — über Mail-Verlauf hinausgehend, inkl. vergangener Termine und Notizen — und schickt sie als eigene Telegram-Nachricht.

### Priorität 2: Reklamations- oder Eskalations-Mails in der Nacht

**Auslöser:** Mail mit Keyword-Treffern aus der Heartbeat-Regel (Reklamation, dringend, Ausfall, Produktionsstopp) von einem bekannten Kunden, die seit Mitternacht eingegangen ist.

**Vorgeschlagene Aktion:**
```
Entwurf für Antwort an [Absender]
```

**Umsetzung bei Zustimmung:** Der Assistent erstellt einen Mail-Antwort-Entwurf, der auf die konkrete Anfrage eingeht und höflich Rückfragen stellt, wo Informationen fehlen. Entwurf landet im Chat zum Kopieren oder zum weiteren Bearbeiten — nicht automatisch versendet.

### Priorität 3: Montag + überfällige Leads

**Auslöser:** Wochentag = Montag UND mindestens ein Lead im Briefing-Leads-Abschnitt aufgeführt.

**Vorgeschlagene Aktion:**
```
Follow-Up-Mail an [Name des obersten Leads]
```

**Umsetzung bei Zustimmung:** Entwurf einer freundlichen Nachfass-Mail, der den letzten Sachstand aus `lead-tracker/data/leads.csv` aufgreift und eine konkrete Rückfrage enthält.

### Priorität 4: Terminkonflikt heute

**Auslöser:** Zwei Termine am gleichen Tag überschneiden sich zeitlich.

**Vorgeschlagene Aktion:**
```
Löse Konflikt zwischen [Termin 1] und [Termin 2]
```

**Umsetzung bei Zustimmung:** Der Assistent prüft, welcher Termin Vorrang hat (extern > intern, Kunde > eigenes), schlägt dem Nutzer vor, welchen er verschieben sollte, und — wenn der Nutzer bestätigt — erstellt einen Entschuldigungs-/Verschiebungs-Mail-Entwurf.

## Priorisierung

Wenn mehrere Aktionen in Frage kommen: Maximal drei werden angezeigt, in dieser Reihenfolge:

1. Aktion aus Priorität 1 (externer Termin heute)
2. Aktion aus Priorität 2 (Eskalations-Mail)
3. Aktion aus Priorität 3 oder 4 (Leads / Konflikte)

Wenn keine Auslöser aus 1–4 passen: Optionen-Abschnitt weglassen. Ein Briefing ohne Aktionen ist nicht "leer" — es ist ein entspannter Tag.

## Aktionen, die wir bewusst NICHT anbieten

- **Keine "Wetter anschauen"-Aktionen.** Zu generisch. Wenn der Nutzer Wetter will, fragt er.
- **Keine "News-Zusammenfassung".** Gehört nicht ins Business-Briefing, führt zu Ablenkung.
- **Keine Aktionen zu internen Meetings.** Interne Abstimmungen bereitet der Nutzer selbst vor oder jemand vom Team macht es.
- **Keine "Stelle mir folgende Fragen"-Rückspiel-Aktionen.** Der Briefing-Skill ist für Hinlegen da, nicht für Dialoge.

## Bei Zweifeln

Im Zweifel lieber eine Aktion weniger als eine zu viel. Ein Briefing mit einer starken, passenden Option ist besser als drei mittelmäßige Optionen.
