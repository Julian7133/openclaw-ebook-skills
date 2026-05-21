# Kapitel 8: MEMORY.md — Das Gedächtnis

## MEMORY.md erstellen

```bash
nano ~/.openclaw/workspace/MEMORY.md
```

## MEMORY.md Minimal-Template

```markdown
# MEMORY.md

Mein Name ist [DEIN NAME].
Meine Firma ist [DEINE FIRMA].
```

**Hinweis:** Dreaming füllt diese Datei automatisch mit relevanten Einträgen.

Speichern: `Strg+O` → Enter → `Strg+X`

## memory/ Verzeichnis prüfen (automatisch)

```bash
ls -la ~/.openclaw/workspace/memory/
# Sollte tägliche Dateien enthalten (z.B. 2026-04-29.md)
```

## Dreaming aktivieren (automatische Memory-Pflege)

```bash
nano ~/.openclaw/openclaw.json
```

Füge dieses Konfigurations-Snippet hinzu:
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

## Manuelle Memory-Einträge (optional)

```markdown
## Wichtige Entscheidungen

- Seit [DATUM]: [Entscheidung]
- [Weitere wichtige Punkte]

## Kontakte & Beziehungen

- [Name]: [Rolle/Kontext]
```

## Gateway neu starten

```bash
openclaw gateway restart
```

---

**Dreaming erklärt im Buch.**  
Updates auf: https://berlow.de/openclaw-material