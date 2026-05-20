# Kapitel 5: Backup der Konfiguration

Copy-Paste-Blatt für Git-Backups im OpenClaw-Workspace und einfache Wiederherstellung.

## Git-Repository initialisieren

```bash
cd ~/.openclaw
git init
git add .
git commit -m "Funktionierender Grundzustand"
```

## GitHub-Remote hinzufügen (optional, empfohlen)

Auf GitHub ein neues Repository erstellen, dann auf dem Server:

```bash
git remote add origin https://github.com/IHR_USERNAME/IHR_REPO.git
git branch -M main
git push -u origin main
```

## Nach größeren getesteten Änderungen sichern

Vor dem Commit sollte `openclaw gateway restart` erfolgreich gewesen sein.

```bash
git add .
git commit -m "Beschreibung der Änderung"
git push
```

## Letzten funktionierenden Zustand wiederherstellen

```bash
git checkout -- .
```

## Config-Backup vor manuellen Änderungen

Vor manuellen Änderungen an `openclaw.json` oder `.env`:

```bash
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak
```

Bei Fehlern vergleichen:

```bash
diff ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak
```

Keine Ausgabe bedeutet: keine Unterschiede.

## Kapitel-Check

- `~/.openclaw` ist ein Git-Repository
- Der funktionierende Grundzustand ist committed
- Optional: Remote ist verbunden und `git push` funktioniert
- Vor riskanten Config-Änderungen existiert eine `.bak`-Datei

---

**Erklärungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides
