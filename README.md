# OpenClaw eBook Skills

Begleitrepository zum Buch *„OpenClaw für den Mittelstand"* von Julian Berlow.

Enthält **Kopier-Vorlagen, Befehle und Skills** für alle Kapitel des Buches.

## 🎯 Zweck dieses Repos

**Buch + GitHub = Maximale Effizienz**

1. **Im Buch lesen** Sie die Erklärungen, Kontexte und Entscheidungen
2. **Von GitHub kopieren** Sie die Befehle, Templates und Master-Prompts
3. **Kein Abtippen** komplexer Konfigurationen

## 📁 Repo-Struktur (überarbeitet)

```
.
├── README.md                    — Diese Datei
├── de/
│   ├── README.md                — Deutsche Übersicht
│   ├── setup/                   # 🔥 NEU: Kapitel 3–12
│   │   ├── README.md            — Setup-Anleitung
│   │   ├── KAPITEL-03.md        # Server mieten & absichern
│   │   ├── KAPITEL-04.md        # OpenClaw installieren
│   │   ├── KAPITEL-05.md        # Backup mit Git
│   │   ├── KAPITEL-06.md        # SOUL.md Template
│   │   ├── KAPITEL-07.md        # USER.md Template
│   │   ├── KAPITEL-08.md        # MEMORY.md Template
│   │   ├── KAPITEL-09.md        # IDENTITY.md & AGENTS.md
│   │   ├── KAPITEL-10.md        # HEARTBEAT.md Template
│   │   ├── KAPITEL-11.md        # E-Mail-Triage Master-Prompt
│   │   └── KAPITEL-12.md        # Kalender-Assistent Master-Prompt
│   └── skills/                  # Kapitel 13–15 (Business-Workflows)
│       ├── lead-tracker/        # Kapitel 13: Lead-Liste + Follow-Up
│       ├── daily-briefing/      # Kapitel 14: Morgen-Briefing
│       └── market-radar/        # Kapitel 15: Wettbewerbs- und Branchen-Radar
└── en/
    ├── README.md                — English overview
    └── skills/                  — English skills (in progress)
```

## 🚀 Verwendung

### Für Kapitel 3–12 (Setup & Konfiguration)

1. **Lesen Sie im Buch** das entsprechende Kapitel
2. **Gehen Sie zu** `de/setup/KAPITEL-XX.md`
3. **Kopieren Sie** die Befehle/Templates/Master-Prompts
4. **Führen Sie sie aus** in Ihrer Umgebung

**Beispiel für Kapitel 11:**
```bash
# 1. Im Buch Kapitel 11 lesen
# 2. In KAPITEL-11.md den Master-Prompt kopieren
# 3. Platzhalter ersetzen
# 4. Prompt an OpenClaw-Assistenten senden
```

### Für Kapitel 13–15 (Business-Workflows)

Das Buch beschreibt die Installation im jeweiligen Kapitel. Die Kurzversion:

```bash
cd ~/.openclaw/workspace
git clone https://github.com/Julian7133/openclaw-ebook-skills.git begleit-repo
cp -r begleit-repo/de/skills/lead-tracker skills/
```

Anschließend eine neue OpenClaw-Session starten, damit der Skill erkannt wird.

## 🔥 Besonderheit: Master-Prompts

Die Kapitel **11 und 12** enthalten **vollständige Master-Prompts**. Diese können Sie:

1. Direkt an Ihren OpenClaw-Assistenten senden
2. Der Assistent richtet dann **alles automatisch ein**
3. Kein manuelles Konfigurieren notwendig

## 📚 Buch-Integration

- **Buch:** Vollständige Erklärungen, Beispiele, Kontext
- **GitHub:** NUR Befehle/Templates zum Kopieren
- **Website:** Updates und Ergänzungen (berlow.de/openclaw-guides)

## 🔄 Updates

### Setup-Dateien (Kapitel 3–12)
Wenn Setup-Dateien aktualisiert werden (im Repo), können Sie sie direkt von GitHub kopieren.

### Skills (Kapitel 13–15)
Wenn ein Skill nach der Installation aktualisiert wird, kann der Leser mit `git pull` im `begleit-repo/` die neuen Dateien ziehen. Die bereits kopierten Skills unter `~/.openclaw/workspace/skills/` bleiben dabei unverändert — Änderungen werden bewusst per `cp` übernommen, sodass eigene Anpassungen (z.B. an den References-Dateien) nicht überschrieben werden.

## 🌐 Online-Ressourcen

- **Buch-Updates:** https://berlow.de/openclaw-guides
- **GitHub Issues:** Für Fragen und Verbesserungsvorschläge
- **Community:** Discord-Community für OpenClaw

## 📄 Lizenz

MIT.