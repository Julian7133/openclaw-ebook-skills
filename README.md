# OpenClaw eBook Skills

Begleitrepository zum Buch *„OpenClaw für den Mittelstand"* von Julian Berlow.

Enthält die OpenClaw-Skills, die in den Kapiteln 11–15 des Buches schrittweise verwendet werden. Die Skills sind so aufgebaut, dass sie direkt in ein laufendes OpenClaw-Setup kopiert werden können.

## Repo-Struktur

```
.
├── README.md
├── de/
│   ├── README.md              — deutsche Übersicht
│   └── skills/
│       ├── lead-tracker/      — Kapitel 13: Lead-Liste + Follow-Up
│       │   ├── SKILL.md
│       │   ├── references/
│       │   └── data/
│       ├── daily-briefing/    — Kapitel 14: Morgen-Briefing
│       │   ├── SKILL.md
│       │   └── references/
│       └── market-radar/      — Kapitel 15: Wettbewerbs- und Branchen-Radar
│           ├── SKILL.md
│           ├── references/
│           └── data/
└── en/
    ├── README.md              — English overview
    └── skills/                — English skills (in progress)
```

## Installation

Das Buch beschreibt die Installation im jeweiligen Kapitel. Die Kurzversion:

```bash
cd ~/.openclaw/workspace
git clone https://github.com/Julian7133/openclaw-ebook-skills.git begleit-repo
cp -r begleit-repo/de/skills/lead-tracker skills/
```

Anschließend eine neue OpenClaw-Session starten, damit der Skill erkannt wird.

## Updates

Wenn ein Skill nach der Installation aktualisiert wird (im Repo), kann der Leser mit `git pull` im `begleit-repo/` die neuen Dateien ziehen. Die bereits kopierten Skills unter `~/.openclaw/workspace/skills/` bleiben dabei unverändert — Änderungen werden bewusst per `cp` übernommen, sodass eigene Anpassungen (z.B. an den References-Dateien) nicht überschrieben werden.

## Lizenz

MIT.
