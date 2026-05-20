# Kapitel 8: MEMORY.md — Das Gedächtnis

Copy-Paste-Blatt für Langzeitgedächtnis, Dreaming und erste Tests.

## MEMORY.md erstellen

```bash
nano ~/.openclaw/workspace/MEMORY.md
```

## Minimalvorlage

```markdown
# MEMORY.md

Mein Name ist [IHR NAME].
Meine Firma ist [FIRMA].
```

**Hinweis:** Dreaming füllt diese Datei automatisch mit relevanten Einträgen.

Optional für wichtige manuelle Einträge:

```markdown
## Wichtige Entscheidungen

- Seit 15.03.2026: Keine Aufträge unter 500 Euro.
- Marketing-Texte ab sofort in der „Du"-Form.
```

Speichern: `Strg+O` → Enter → `Strg+X`

## memory/ Verzeichnis prüfen (automatisch)

```bash
ls -la ~/.openclaw/workspace/memory/
```

Sollte tägliche Dateien enthalten (z. B. `2026-04-29.md`).

## Dreaming aktivieren

In Telegram:

```text
/dreaming on
```

Weitere Befehle:

```text
/dreaming status
/dreaming off
/dreaming help
```

Alternative über `openclaw.json`:

```bash
nano ~/.openclaw/openclaw.json
```

Block einfügen, falls `memory-core` noch nicht konfiguriert ist:

```json
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true
          }
        }
      }
    }
  }
}
```

Gateway neu starten:

```bash
openclaw gateway restart
```

Memory-Status prüfen:

```bash
openclaw memory status
```

## Erster Test

Dem Assistenten schreiben:

```text
Mein wichtigster Kunde ist die TechSupply GmbH. Die Ansprechpartnerin heißt Frau Meyer und sie will alle Angebote als PDF.
```

Nach kurzer Wartezeit fragen:

```text
Was weißt du über meine wichtigsten Kunden?
```

Falls der Test nicht klappt:

```bash
openclaw memory status
openclaw gateway restart
```

## Kapitel-Check

- `~/.openclaw/workspace/MEMORY.md` existiert
- Dreaming ist per Telegram oder Config aktiviert
- `openclaw memory status` zeigt einen aktiven Memory-Index oder eine klare Fehlermeldung
- Der Test-Fakt ist für den Assistenten wiederauffindbar

---

**Dreaming erklärt im Buch.**  
Updates auf: https://berlow.de/openclaw-guides
