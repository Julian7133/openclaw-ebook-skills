# Kapitel 4: OpenClaw installieren

## OpenRouter API-Schlüssel (optional)
1. Auf [openrouter.ai](https://openrouter.ai) gehen
2. Account erstellen oder einloggen
3. Auf "Get API Key" klicken
4. Schlüssel benennen (z.B. `OpenClaw Agent`)
5. Key kopieren und sicher speichern (beginnt mit `sk-or-v1-...`)

## OpenClaw installieren (Variante A: Manuell)

```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
export PATH="/home/deinname/.npm-global/bin:$PATH"
openclaw --version
echo 'export PATH="/home/deinname/.npm-global/bin:$PATH"' >> ~/.bashrc
sudo reboot
# Nach Reboot wieder einloggen
```

## Onboarding & Telegram Bot

```bash
openclaw onboard --install-daemon
```

Folge dem Wizard:
1. **Provider:** OpenRouter API Key einfügen
2. **Default model:** z.B. `openrouter/anthropic/claude-sonnet-4.6`
3. **Select a channel:** `telegram`
4. **Telegram bot token:** Von @BotFather holen
5. **Brave Search API Key:** Auf [brave.com/search/api](https://brave.com/search/api) generieren
6. **Configure Skills now:** `no`
7. **Enable hooks:** `no`

## Konfiguration absichern

### 1. Tokens in .env Datei auslagern

```bash
nano ~/.openclaw/openclaw.json
# Alle Tokens kopieren
```

Lokale Datei erstellen mit:
```
OPENCLAW_GATEWAY_TOKEN="dein_gateway_token"
OPENCLAW_TELEGRAM_TOKEN="dein_telegram_token"  
OPENCLAW_BRAVE_SEARCH_TOKEN="dein_brave_token"
```

```bash
nano ~/.openclaw/.env
# Inhalt einfügen und speichern (Strg+O → Enter → Strg+X)
```

### 2. openclaw.json anpassen

```bash
nano ~/.openclaw/openclaw.json
```

Ändere jedes `"token": "wert"` zu:
```json
"token": {
  "source": "env",
  "provider": "default", 
  "id": "OPENCLAW_GATEWAY_TOKEN"
}
```

### 3. Gateway neu starten und prüfen

```bash
openclaw gateway restart
openclaw gateway status
openclaw gateway status --deep
openclaw doctor
```

## Claude Code Installation (Variante B: KI-gestützt)

### 1. Claude Code installieren

```bash
npm install -g @anthropic-ai/claude-code
cd ~/.openclaw/
claude
# Link im Browser öffnen und verbinden
```

### 2. Prompt für Token-Absicherung

```
Du bist mein Serveradministrator. Ich habe OpenClaw gerade per Onboarding-Wizard eingerichtet.

1. Lies ~/.openclaw/openclaw.json und identifiziere alle Tokens
2. Lege ~/.openclaw/.env an mit VARIABLENNAME="wert"
3. Ersetze Tokens in openclaw.json durch Umgebungsvariablen-Referenzen
4. Führe openclaw doctor aus und berichte Ergebnis
```

## Troubleshooting

```bash
# Gateway nicht läuft?
openclaw gateway restart

# Port-Konflikt?
openclaw gateway status --deep

# Onboarding fehlgeschlagen?
openclaw doctor
```

---

**Erklärungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides