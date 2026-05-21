# Quellen für den Markt-Radar

Diese Datei definiert, welche Websites der Radar wöchentlich prüft. Zwei Kategorien: direkte Wettbewerber und Branchenquellen.

Passen Sie diese Liste auf Ihr eigenes Umfeld an — die unten aufgeführten Einträge sind Thomas Webers Setup (Maschinenbau-Zulieferer, Präzisionsteile) und dienen als Beispiel.

## Direkte Wettbewerber

Pro Eintrag: Domain, kurzer Kontext, und welche Rubrik primär interessiert.

| Domain | Kontext | Fokus |
|--------|---------|-------|
| sittner-präzision.example | Regionaler Hauptwettbewerber, ähnliche Größe | Pressemitteilungen, Stellenausschreibungen |
| gebr-meier-metall.example | Größerer Player, oft Preisführer | Produkt-News, Kapazitätsmeldungen |
| novaflex-beschichtung.example | Komplementär/Wettbewerber bei Oberflächenbehandlung | Neue Verfahren |
| stahlwerk-augsburg.example | Aufsteigender Wettbewerber seit 2024 | Alle öffentlichen Neuigkeiten |
| rapidform-gmbh.example | Wettbewerber bei Kleinserien | Stellenausschreibungen (Indikator für Wachstum), Produkt-News |

**Hinweis zu Stellenausschreibungen:** Diese sind ein überraschend guter Indikator für die Geschäftslage eines Wettbewerbers. Wer plötzlich fünf neue Zerspaner sucht, plant Kapazitäts-Ausbau. Wer seit sechs Monaten nichts mehr postet, steht möglicherweise unter Druck.

## Branchenquellen

| Domain | Typ | Fokus |
|--------|-----|-------|
| maschinenmarkt.vogel.de | Fachmedium | Neue Technologien, Marktanalysen |
| industrie.de | Fachmedium | Automotive-Lieferkette, Deutschland-Fokus |
| vdma.org | Branchenverband | Statistiken, Positionspapiere, Verbandsnews |
| zeitschrift-wt-werkstattstechnik.de | Fachzeitschrift | Zerspanung, Präzisionstechnik |

## Regulatorische Quellen (optional)

Aktuell nicht aktiviert. Wenn Sie Änderungen in Normen, Zollrecht oder Lieferkettengesetz verfolgen möchten, fügen Sie hier Quellen hinzu, z.B.:

- `din.de` — DIN-Normen
- `zoll.de` — Zoll- und Außenwirtschaft
- `bmas.de` — Sozial-, Arbeits- und Lieferketten-Recht

Wenn dieser Abschnitt Einträge enthält, scannt der Radar auch diese; wenn er leer bleibt, bleiben sie weg.

## Format der Suche pro Quelle

Der Skill bildet aus jedem Eintrag automatisch eine Suchanfrage. Für Wettbewerber typischerweise:

```
site:<domain> news OR neuigkeiten OR pressemitteilung OR jobs
```

Für Branchenmedien kombiniert er die Domain mit den Keywords aus `topics.md`:

```
site:<domain> <topic-keyword-1> OR <topic-keyword-2>
```

Wenn Sie für eine Quelle eine andere Suchstrategie wollen (z.B. eine RSS-URL statt `site:`-Suche), fügen Sie eine Zeile „Benutzerdefinierte Query:" unter den Tabelleneintrag ein und formulieren Sie die Suche explizit.

## Pflege der Liste

Die Liste entwickelt sich. Neue Wettbewerber ergänzen Sie per Chat:

> „Füge hinzu: formtec-süd.example, mittelständischer Wettbewerber bei Tiefziehteilen, Fokus auf Produktnews."

Der Assistent passt diese Datei entsprechend an. Analog für Entfernen:

> „Entferne stahlwerk-augsburg.example aus dem Radar — die sind inzwischen in einen anderen Markt abgewandert."
