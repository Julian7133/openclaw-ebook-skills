# Kapitel 5: Backup

## Git Repository initialisieren

```bash
cd ~/.openclaw
git init
git add .
git commit -m "Funktionierender Grundzustand"
```

## GitHub hinzufügen (optional)

```bash
git remote add origin https://github.com/DEIN_USERNAME/DEIN_REPO.git
git branch -M main
git push -u origin main
```

## Bei Änderungen committen

```bash
git add .
git commit -m "Beschreibung der Änderung"
git push
```

## Wiederherstellung bei Problemen

```bash
git checkout -- .
```

## Config-Backup

```bash
# Vor Änderungen:
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak

# Nach Änderungen prüfen:
diff ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak
# Keine Ausgabe = keine Unterschiede
```

---

**Erklärungen im Buch.**  
Updates auf: https://berlow.de/openclaw-material