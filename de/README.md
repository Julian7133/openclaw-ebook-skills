# Skills zum deutschen eBook

OpenClaw-Skills, die in den Kapiteln 11–15 des Buches schrittweise aufgebaut und verwendet werden.

## Verfügbare Skills

| Skill | Kapitel | Zweck |
|-------|---------|-------|
| `lead-tracker` | 13 | Persönliche Lead-Liste mit wöchentlichem Follow-Up-Review |
| `daily-briefing` | 14 | Kompaktes Morgen-Briefing mit Termin-Prep und Aktionsvorschlägen |
| `market-radar` | 15 | Wöchentlicher Wettbewerbs- und Branchen-Radar mit Deduplizierung |

Weitere Skills werden mit den folgenden Kapiteln ergänzt.

## Installation eines Skills

Aus dem Repo-Root:

```bash
cd ~/.openclaw/workspace
git clone https://github.com/Julian7133/openclaw-ebook-skills.git begleit-repo
cp -r begleit-repo/de/skills/<skill-name> skills/
```

Anschließend eine neue OpenClaw-Session starten.

## Aufbau jedes Skills

Jeder Skill folgt demselben Muster:

- `SKILL.md` — Haupt-Definition, die OpenClaw beim Triggering lädt
- `references/` — Detail-Regeln, die bei Bedarf nachgeladen werden
- `data/` — Arbeitsdaten (leer als Template)

Der Nutzer kann jede Datei in `references/` direkt bearbeiten oder den Assistenten bitten, sie anzupassen. Das ist bewusst so gemacht: die Skill-Logik ist transparent und vom Nutzer formbar.
