# Deduplizierungs-Regeln

Verhindert, dass dieselbe Information Woche für Woche neu auftaucht. Die Datei `./data/seen-urls.txt` führt eine fortlaufende Liste aller URLs, die in vergangenen Reports gelandet sind.

## Format der Datei

Eine URL pro Zeile, in der Reihenfolge des Eintragens (neueste ganz unten). Keine Timestamps, keine Kommentare — rein eine sortierte Referenzliste.

Beispiel:
```
https://sittner-präzision.example/news/2026/04/werkshalle-ii
https://vdma.org/positionen/2026/lieferketten-novelle
https://industrie.de/automotive/supply-chain-2026-q2
https://sittner-präzision.example/karriere/cnc-dreher-7
```

## URL-Normalisierung vor dem Abgleich

Vor dem Vergleich gegen `seen-urls.txt` werden URLs leicht normalisiert, damit kosmetische Unterschiede nicht zu Duplikaten führen:

- Tracking-Parameter entfernen: `?utm_source=...`, `?utm_medium=...`, `?ref=...`, `?fbclid=...`
- Trailing-Slashes normalisieren: `example.com/page/` und `example.com/page` gelten als gleich
- Anker-Fragmente entfernen: `example.com/page#abschnitt-3` gilt als `example.com/page`
- Protokoll egal: `http://` und `https://` gleich behandeln

Die normalisierte Form ist das, was in `seen-urls.txt` landet.

## Was gehört in die Datei

- URLs, die im Report abgedruckt wurden (Wettbewerber- und Branchenfunde)
- Nichts sonst. Nicht-verwendete Suchtreffer bleiben draußen — siehe Begründung in der SKILL.md („Wenn ein Artikel nächste Woche in besserem Kontext auftaucht…").

## Was gehört NICHT in die Datei

- Fehler-URLs (404, Timeout) — die sollen beim nächsten Versuch wieder erkannt werden
- URLs von Startseiten oder Übersichtsseiten (nur Detail-Artikel dedupen)
- Suchergebnis-URLs von Brave selbst

## Größenbegrenzung

Die Datei wächst mit der Zeit. Nach rund drei Jahren sind typische Größen erreicht:
- Bei 5 Wettbewerbern und 3 Branchenquellen mit ~2 neuen Funden pro Woche: ~500 URLs/Jahr, ~1.500 nach drei Jahren.
- Das ist textuell unproblematisch — die Datei bleibt unter 100 kB.

Trotzdem: Einträge älter als 2 Jahre können periodisch entfernt werden, wenn der Nutzer es wünscht. Dafür merkt sich der Skill nicht selbständig, wann ein Eintrag erstellt wurde — der Nutzer muss explizit anweisen („Lösche in seen-urls.txt alle Einträge, die 2024 oder früher gesammelt wurden" funktioniert nur, wenn wir Einträge optional mit einem Kommentar-Datum versehen; standardmäßig haben wir das nicht).

Wenn Sie Datum-Stempel wollen, passen Sie das Format auf:
```
https://... # 2026-04-22
```
an, und rekonfigurieren Sie den Skill, die Stempel zu ignorieren beim Abgleich. Das ist bewusst als optionale Erweiterung vorgesehen — für den Standardfall genügt die schlichte Liste.

## Integrität der Datei

Wenn `seen-urls.txt` versehentlich beschädigt wird (manuell editiert, abgeschnitten, leer): Nicht automatisch wiederherstellen. Der nächste Radar-Lauf behandelt dann alle Funde der aktuellen Suche als neu und produziert einen ungewöhnlich langen Report. Das ist unschön, aber unschädlich — die Datei ist kein Datenverlust, nur eine Effizienz-Optimierung.

Wenn der Nutzer die Datei bewusst zurücksetzt („Ich will einmal wieder alles frisch sehen"): `seen-urls.txt` auf leer setzen und dem Nutzer bestätigen, dass beim nächsten Lauf alle aktuellen Funde als neu gelten.
