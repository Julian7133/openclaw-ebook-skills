---
name: market-radar
description: Wöchentlicher Markt- und Wettbewerbs-Radar. Scannt eine definierte Liste von Wettbewerber-Websites und Branchenquellen, filtert nach den eigenen Kernthemen und erstellt einen kompakten Report mit Deduplizierung gegenüber Vorwochen. Wird aktiv bei Anfragen wie "führe meinen Markt-Radar aus", "was gibt es Neues bei meinen Wettbewerbern?", "starte die wöchentliche Branchenrecherche". Standardmäßig per Cron jeden Freitag um 07:30 ausgelöst.
---

# Market-Radar

Führt eine strukturierte, wiederkehrende Markt- und Wettbewerbs-Recherche durch. Der Skill ist bewusst spezifisch aufgebaut (nicht generische Web-Suche), weil er eine bestimmte Aufgabe optimal lösen soll: jede Woche aus einer festgelegten Quellenliste genau das herausziehen, was für das eigene Geschäft relevant und neu ist.

## Ausführungskontext

Dieser Skill läuft in einer isolierten Session (ähnlich wie der Lead-Review aus dem lead-tracker-Skill). Grund: Der Lauf produziert viele Tool-Calls (Web-Suchen und -Fetches), die den Hauptkontext ansonsten stark füllen würden. Das fertige Ergebnis wird als Datei in `./data/reports/` abgelegt und per Telegram-Kurzfassung angekündigt — Nachfragen dazu stellt der Nutzer später im Main-Thread, dort kann der Assistent die Report-Datei jederzeit laden.

## Ablauf

Wenn das Radar ausgelöst wird (per Cron oder manuell):

1. Lade `references/sources.md` — die Liste der zu beobachtenden Wettbewerber und Branchenquellen.
2. Lade `references/topics.md` — die Kernthemen, nach denen gefiltert werden soll.
3. Lade `references/report-format.md` — die Struktur der Report-Datei und der Telegram-Kurzfassung.
4. Lade `references/seen-urls-rules.md` — die Deduplizierungs-Regeln.
5. Für jede Quelle: Suche und fetche (siehe unten).
6. Filtere die gefundenen Elemente nach Themen-Relevanz und prüfe gegen `./data/seen-urls.txt`.
7. Schreibe den vollen Report nach `./data/reports/market-radar-YYYY-MM-DD.md`.
8. Ergänze neue URLs in `./data/seen-urls.txt`.
9. Poste die Telegram-Kurzfassung mit Verweis auf den Report.

### Suche und Fetch

Für jede Wettbewerber-Website in `sources.md`:
- Nutze `web_search` mit der Firmen-Domain als `site:`-Operator und einem Zeitfilter der letzten 10 Tage (damit auch am Feiertagsmontag nichts durchrutscht).
- Beispiel-Query: `site:sittner-präzision.de news OR neuigkeiten OR pressemitteilung`.
- Wenn interessante Treffer dabei sind: `web_fetch` auf die entsprechenden URLs.

Für jede Branchen-Quelle in `sources.md` (Medien, Verbände):
- Nutze `web_search` mit der Quellen-Domain plus den relevantesten Themen aus `topics.md`.
- Bei gefundenen Artikeln: `web_fetch` für den vollen Text.

**Wichtig zur technischen Einschränkung:** `web_fetch` führt kein JavaScript aus. Wenn eine Website nur JS-gerendert lädt, liefert der Fetch leere oder unvollständige Inhalte. In dem Fall: Die Brave-Snippets aus der Suche als Fallback nutzen (Titel und Kurz-Description reichen für die Report-Zeile). Nicht versuchen, den vollständigen Text zu erraten.

### Themenfilterung

Ein Fund wird nur in den Report aufgenommen, wenn:
- Sein Titel oder Snippet mindestens einen Keyword-Treffer gegen `topics.md` hat, ODER
- Er in einer Rubrik steht, die in `topics.md` als „alles-relevant" markiert ist (z.B. „alle Pressemitteilungen direkter Wettbewerber").

Irrelevante Funde werden komplett verworfen — nicht erwähnt, nicht in `seen-urls.txt` eingetragen. Letzteres ist bewusst: Wenn ein Artikel nächste Woche in besserem Kontext auftaucht, soll er nicht deshalb übersprungen werden, weil er damals in einem zu weiten Scan mitgelaufen ist.

### Deduplizierung

Lade `./data/seen-urls.txt`. Wenn die URL eines Fundes dort bereits existiert: überspringen. Sonst: in den Report aufnehmen. Nach dem Report-Schreiben neue URLs anhängen.

Details zum Format der Datei siehe `references/seen-urls-rules.md`.

## Fehlertoleranz

- **Eine Quelle nicht erreichbar:** Die Quelle im Report als „nicht erreichbar" markieren, Lauf für alle anderen Quellen fortsetzen.
- **Brave Rate-Limit getroffen:** Lauf pausieren, beim nächsten planmäßigen Zeitpunkt neu versuchen. Nicht auf eine andere Quelle ausweichen — das verzerrt die Statistik, welche Quellen aktiv sind.
- **Keine neuen Funde in der ganzen Woche:** Trotzdem eine kurze Telegram-Nachricht schicken („Markt-Radar KW XX: nichts Neues"). Das ist wichtig, damit der Nutzer den Skill als verlässlich wahrnimmt — nicht als „hat der diese Woche gelaufen?".

## Anpassungen durch den Nutzer

| Wunsch des Nutzers | Zu bearbeitende Datei |
|--------------------|----------------------|
| Wettbewerber hinzufügen/entfernen | `references/sources.md` |
| Branchenmedien oder Verbände ergänzen | `references/sources.md` |
| Themen-Keywords ändern | `references/topics.md` |
| Layout des Reports anpassen | `references/report-format.md` |
| Deduplizierung lockern/verschärfen | `references/seen-urls-rules.md` |
| Cron-Zeit ändern | Cron-Expression in `~/.openclaw/cron/jobs.json` |

Bei Änderungen an `sources.md` oder `topics.md` dem Nutzer das aktuelle Stand nach der Änderung kurz bestätigen („Neue Quellenliste: 6 Wettbewerber, 3 Branchenmedien").

## Report-Archivierung

Reports bleiben unbegrenzt in `./data/reports/` liegen. Faustregel: Reports älter als 12 Monate können per Chat-Anweisung entfernt werden („Lösche alle Market-Radar-Reports aus 2025"). Nicht automatisch löschen — manchmal sind ältere Einträge für rückblickende Analysen wertvoll.
