# Report-Format

Der Markt-Radar produziert zwei Ausgaben pro Lauf: eine ausführliche Markdown-Datei im Workspace und eine kompakte Telegram-Nachricht, die darauf verweist.

## 1. Die Report-Datei

Liegt unter `./data/reports/market-radar-YYYY-MM-DD.md` (Datum des Laufs).

### Struktur

```markdown
# Markt-Radar KW [Kalenderwoche], [Datum]

Zeitraum: letzte [N] Tage. [M] Quellen geprüft, [K] neue Funde, [J] nach Filter übrig.

## Wettbewerber

### [Firmenname]
[Für jeden neuen Fund:]
- **[Titel]** ([Datum des Artikels])
  [2–3-Zeilen-Zusammenfassung]
  Quelle: [URL]

Wenn nichts Neues: „_Keine neuen Meldungen seit dem letzten Radar._"

## Branche

### [Quellenname]
[Für jeden neuen Fund:]
- **[Titel]** ([Datum])
  [2–3-Zeilen-Zusammenfassung, mit Bezug zu den getroffenen Kernthemen]
  Quelle: [URL]

## Nicht erreicht

Falls einzelne Quellen im aktuellen Lauf technisch nicht erreichbar waren:
- [Quelle] — [Fehlerursache, knapp: Timeout, 403, etc.]

## Zusammenfassung

[3–6 Zeilen, was die wichtigsten Befunde sind, plus ggf. eine Frage:
„Bei Sittner ist eine neue Werkshalle in Bau angekündigt — will der Nutzer
dazu mehr recherchieren lassen?"]
```

### Stilprinzipien für die Zusammenfassungen

- Zwei bis drei Zeilen pro Fund. Genau das, was der Nutzer wissen muss, ohne die Quelle zu klicken.
- Nicht paraphrasieren, was jeder ohnehin weiß. Wenn eine Pressemitteilung nur sagt „wir freuen uns, unser Jubiläum zu feiern" — weglassen oder einzeilig abhandeln.
- Bei Stellenausschreibungen: die *Zahl* und das *Profil* in den Vordergrund. „5 Zerspaner gesucht, Schwerpunkt CNC-Dreher" ist informativer als „neue Stelle ausgeschrieben".
- Bei Preisnennungen: ausdrücklich machen. Wenn nichts Konkretes dazu stand, weglassen.

### Was nicht in die Datei gehört

- Kein generischer Rahmentext („In dieser Woche hat das Radar wieder einen spannenden Blick in Ihre Branche geworfen…").
- Keine Werbung für den Skill selbst.
- Keine Spekulationen. Wenn die Quelle sagt „mögliche Übernahme": das als mögliche Übernahme weitergeben, nicht als Fakt.
- Keine Wiederholung des Inhalts der Vorwoche. Die Deduplizierung erledigt das eigentlich, aber falls ein identischer Artikel unter einer anderen URL auftaucht: aus dem Report streichen.

## 2. Die Telegram-Kurzfassung

Formal:

```
🎯 Markt-Radar KW [XX], [Datum]

[Zahl] neue Funde aus [Zahl] Quellen.

Das Wichtigste:
• [Top-Fund 1]
• [Top-Fund 2]
• [Top-Fund 3, optional]

Voller Report: ~/.openclaw/workspace/skills/market-radar/data/reports/market-radar-[Datum].md
```

### Auswahl der Top-Funde für Telegram

Maximal drei Zeilen, ausgewählt nach dieser Priorität:

1. Wettbewerber-Meldungen mit Kapazitäts- oder Markt-Implikation (neue Werkshalle, große Übernahme, Insolvenz)
2. Regulatorische Änderungen mit Umsetzungs-Pflicht
3. Branchenmeldungen mit konkreter Zahl/Fakt

Wenn weniger als drei solche Spitzen-Funde vorliegen: nur so viele Zeilen, wie es wirklich gibt. Lieber kürzer als aufgefüllt.

### Wenn der Radar nichts findet

```
🎯 Markt-Radar KW [XX]: nichts Neues.

Alle [Zahl] Quellen geprüft, keine Funde über der Relevanzschwelle.
```

Keine Erklärung dafür, warum das so ist — ein ruhiger Markt ist kein Problem, das es zu lösen gäbe.

## Zur Report-Sprache

Die Schreibweise folgt der SOUL.md des Agenten und ist per Default deutsch. Wenn der Nutzer eine englische Quelle fetcht, kann der Report-Eintrag dazu englisch zitieren — aber die Zusammenfassung bleibt deutsch. Das hält den Report homogen lesbar, auch wenn die Quellenlandschaft zweisprachig ist.
