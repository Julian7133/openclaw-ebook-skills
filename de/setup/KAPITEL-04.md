# Kapitel 4: OpenClaw installieren

Copy-Paste-Blatt für Installation, Onboarding, Token-Absicherung und Diagnose. Führen Sie die Befehle auf dem Hetzner-Server aus, sofern nicht anders markiert.

## API-Schlüssel bereithalten

Vor dem Onboarding einen KI-Provider-Schlüssel bereitlegen, z. B. von [OpenRouter](https://openrouter.ai):

1. Auf [openrouter.ai](https://openrouter.ai) gehen
2. Account erstellen oder einloggen
3. Auf „Get API Key“ klicken
4. Schlüssel benennen (z. B. `OpenClaw Agent`)
5. Key kopieren und sicher speichern (beginnt mit `sk-or-v1-...`)

Für Telegram außerdem über `@BotFather` einen Bot erstellen und den Bot-Token bereithalten.

## Variante A: Manuelle Installation

OpenClaw installieren:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

Falls der Installer einen PATH-Export ausgibt, zuerst direkt ausführen. Beispiel:

```bash
export PATH="/home/DEIN_NAME/.npm-global/bin:$PATH"
```

Installation prüfen:

```bash
openclaw --version
```

PATH dauerhaft setzen (Beispiel an den eigenen Pfad anpassen):

```bash
echo 'export PATH="/home/DEIN_NAME/.npm-global/bin:$PATH"' >> ~/.bashrc
```

Falls gefordert, Server neu starten:

```bash
sudo reboot
```

Danach wieder per SSH einloggen.

## Onboarding und Daemon

```bash
openclaw onboard --install-daemon
```

Empfohlene Wizard-Antworten:

- `personal-by-default`: bestätigen
- **Provider:** vorbereiteten API-Schlüssel eintragen
- **Default model:** z. B. `openrouter/anthropic/claude-sonnet-4.6` oder ein aktuelles Medium/High-End-Modell
- **Channel:** `telegram`
- **Telegram bot token:** BotFather-Token einfügen
- **Search Provider:** Brave Search API Key (auf [brave.com/search/api](https://brave.com/search/api) generieren)
- **Configure Skills now:** `no`
- **Enable hooks:** `no`

Danach dem Telegram-Bot `Hi` schreiben. Falls der Bot einen Freigabe-Befehl zurückgibt, diesen auf dem Server ausführen.

## Token-Absicherung manuell

Konfiguration prüfen:

```bash
nano ~/.openclaw/openclaw.json
```

Tokens aus `openclaw.json` in eine Env-Datei übertragen:

```bash
nano ~/.openclaw/.env
```

Vorlage:

```dotenv
OPENCLAW_GATEWAY_TOKEN="<Ihr kopierter Gateway Token>"
OPENCLAW_TELEGRAM_TOKEN="<Ihr kopierter Telegram Token>"
OPENCLAW_BRAVE_SEARCH_TOKEN="<Ihr kopierter Brave Search Token>"
```

Danach in `~/.openclaw/openclaw.json` Klartext-Tokens durch Env-Referenzen ersetzen. Unterstütztes Format neuerer Versionen:

```json
"token": {
  "source": "env",
  "provider": "default",
  "id": "OPENCLAW_GATEWAY_TOKEN"
}
```

Analog für Telegram und Brave Search vorgehen.

Konfiguration testen:

```bash
openclaw gateway restart
openclaw gateway status
openclaw gateway status --deep
openclaw doctor
```

## Gateway-Status und Logs

Diagnose bei Problemen:

```bash
openclaw gateway status --deep
openclaw doctor
openclaw gateway restart
```

Typische Ursachen für `Gateway not running`:

- Onboarding nicht abgeschlossen
- Fehlerhafte Konfiguration in `~/.openclaw/openclaw.json`
- Lokaler Portkonflikt auf `18789`

## Variante B: Claude Code als Serveradministrator

Claude Code auf dem Server installieren:

```bash
npm install -g @anthropic-ai/claude-code
cd ~/.openclaw/
claude
```

Link im Browser öffnen und verbinden.

Prompt für Token-Absicherung:

```text
Du bist mein Serveradministrator. Ich habe OpenClaw gerade per Onboarding-Wizard eingerichtet. Die API-Tokens liegen vermutlich noch als Klartext in ~/.openclaw/openclaw.json. Deine Aufgabe:

1. Lies die Datei ~/.openclaw/openclaw.json und identifiziere alle Tokens (Gateway-Token, Telegram-Token, Brave-Search-Token oder ähnliche).
2. Lege die Datei ~/.openclaw/.env an und trage die gefundenen Tokens dort ein — im Format VARIABLENNAME="wert".
3. Ersetze die Klartext-Tokens in openclaw.json durch Umgebungsvariablen-Referenzen im Format "token": {"source": "env", "provider": "default", "id": "OPENCLAW_GATEWAY_TOKEN"}.
4. Führe danach openclaw doctor aus und berichte mir das Ergebnis.

Erkläre mir jeden Schritt, bevor du ihn ausführst.

Dein Wissen zu OpenClaw ist garantiert unvollständig und veraltet. Bitte nutze daher immer https://docs.openclaw.ai/ sowie insbesondere https://docs.openclaw.ai/start/wizard, um mich durch den Prozess zu führen. Speichere diese Anweisung auch in deiner Claude.md.
```

Prompt für Logs und Debugging:

```text
Führe openclaw logs --follow für 30 Sekunden aus und zeige mir die Ausgabe. Erkläre mir danach, ob es Fehler gibt und wie wir sie beheben.
```

## Kapitel-Check

- `openclaw --version` liefert eine Version
- `openclaw onboard --install-daemon` wurde abgeschlossen
- Telegram-Bot antwortet
- Tokens liegen in `~/.openclaw/.env` statt als Klartext in der Hauptkonfiguration
- `openclaw gateway status --deep` und `openclaw doctor` sind unauffällig oder liefern klare nächste Schritte

---

**Erklärungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides
