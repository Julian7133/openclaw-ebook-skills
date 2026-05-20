# Deutsche Ressourcen zum eBook

Dieser Ordner enthält alle deutschen Ressourcen zum Buch *„OpenClaw für den Mittelstand"* — Copy-Paste-Materialien für Setup (Kapitel 3–12) und Skills (Kapitel 13–15).

## Setup-Kapitel

| Kapitel | Thema |
|---------|-------|
| [3](setup/KAPITEL-03.md) | Server mieten und absichern |
| [4](setup/KAPITEL-04.md) | OpenClaw installieren |
| [5](setup/KAPITEL-05.md) | Backup der Konfiguration |
| [6](setup/KAPITEL-06.md) | `SOUL.md` |
| [7](setup/KAPITEL-07.md) | `USER.md` |
| [8](setup/KAPITEL-08.md) | `MEMORY.md` |
| [9](setup/KAPITEL-09.md) | `IDENTITY.md` & `AGENTS.md` |
| [10](setup/KAPITEL-10.md) | `HEARTBEAT.md` & Cron |
| [11](setup/KAPITEL-11.md) | E-Mail-Triage Master-Prompt |
| [12](setup/KAPITEL-12.md) | Kalender-Assistent Master-Prompt |

## 📁 Struktur

```
de/
├── README.md                    — Diese Datei
├── setup/                       # Kapitel 3–12
│   ├── README.md                — Setup-Anleitung
│   ├── KAPITEL-03.md            # Server mieten & absichern
│   ├── KAPITEL-04.md            # OpenClaw installieren
│   ├── KAPITEL-05.md            # Backup mit Git
│   ├── KAPITEL-06.md            # SOUL.md Template
│   ├── KAPITEL-07.md            # USER.md Template
│   ├── KAPITEL-08.md            # MEMORY.md Template
│   ├── KAPITEL-09.md            # IDENTITY.md & AGENTS.md
│   ├── KAPITEL-10.md            # HEARTBEAT.md Template
│   ├── KAPITEL-11.md            # E-Mail-Triage Master-Prompt
│   └── KAPITEL-12.md            # Kalender-Assistent Master-Prompt
└── skills/                      # Kapitel 13–15 (Business-Workflows)
    ├── lead-tracker/            # Kapitel 13: Lead-Liste + Follow-Up
    ├── daily-briefing/          # Kapitel 14: Morgen-Briefing
    └── market-radar/            # Kapitel 15: Wettbewerbs- und Branchen-Radar
```

## 🎯 Verwendung

### Für Kapitel 3–12 (Setup & Konfiguration)

1. **Lesen Sie im Buch** das entsprechende Kapitel
2. **Gehen Sie zu** `setup/KAPITEL-XX.md`
3. **Kopieren Sie** die Befehle/Templates/Master-Prompts
4. **Führen Sie sie aus** in Ihrer OpenClaw-Umgebung

**Wichtig:** Die Setup-Dateien enthalten **NUR** die Befehle und Templates. Die vollständigen Erklärungen stehen im Buch.

### Für Kapitel 13–15 (Business-Workflows)

1. **Lesen Sie im Buch** das entsprechende Kapitel
2. **Kopieren Sie** den gewünschten Skill in Ihr OpenClaw-Workspace:

```bash
cd ~/.openclaw/workspace
git clone https://github.com/Julian7133/openclaw-ebook-skills.git begleit-repo
cp -r begleit-repo/de/skills/<skill-name> skills/
```

3. **Starten Sie** eine neue OpenClaw-Session, damit der Skill erkannt wird

## 🔥 Besonderheit: Master-Prompts

Die Kapitel **11 und 12** enthalten **vollständige Master-Prompts**. Diese können Sie:

1. Direkt an Ihren OpenClaw-Assistenten senden
2. Der Assistent richtet dann **alles automatisch ein**
3. Kein manuelles Konfigurieren notwendig

## 📚 Verfügbare Skills (Kapitel 13–15)

| Skill | Kapitel | Zweck |
|-------|---------|-------|
| `lead-tracker` | 13 | Persönliche Lead-Liste mit wöchentlichem Follow-Up-Review |
| `daily-briefing` | 14 | Kompaktes Morgen-Briefing mit Termin-Prep und Aktionsvorschlägen |
| `market-radar` | 15 | Wöchentlicher Wettbewerbs- und Branchen-Radar mit Deduplizierung |

## 🔄 Aufbau jedes Skills

Jeder Skill folgt demselben Muster:

- `SKILL.md` — Haupt-Definition, die OpenClaw beim Triggering lädt
- `references/` — Detail-Regeln, die bei Bedarf nachgeladen werden
- `data/` — Arbeitsdaten (leer als Template)

Der Nutzer kann jede Datei in `references/` direkt bearbeiten oder den Assistenten bitten, sie anzupassen. Das ist bewusst so gemacht: die Skill-Logik ist transparent und vom Nutzer formbar.

## 🌐 Online-Ressourcen

- **Buch-Updates:** https://berlow.de/openclaw-guides
- **GitHub:** https://github.com/Julian7133/openclaw-ebook-skills
- **Community:** Discord-Community für OpenClaw

---

**Updates auf:** https://berlow.de/openclaw-guides
