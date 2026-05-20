# Kapitel 11: Workflow 1 - E-Mail-Triage

Diese Seite ist die Kopiervorlage zum Kapitel. Sie richtet den E-Mail-Zugriff so ein, dass OpenClaw Ihre neuen Nachrichten prüfen und Ihnen nur die wichtigen Mails melden kann.

Lesen Sie erst den kurzen Hinweis, dann kopieren Sie die Befehle nacheinander in Ihr Server-Terminal.

## 1. E-Mail-Werkzeug installieren

```bash
sudo apt update
sudo apt install -y himalaya
```

Prüfen Sie danach, ob der Befehl verfügbar ist:

```bash
himalaya --version
```

## 2. Konfigurationsordner anlegen

```bash
mkdir -p ~/.config/himalaya
chmod 700 ~/.config/himalaya
```

## 3. Beispiel-Konfiguration für IONOS anlegen

Ersetzen Sie `ihre-adresse@example.com` durch Ihre echte E-Mail-Adresse. Falls Sie keinen IONOS-Posteingang nutzen, übernehmen Sie die Struktur und tauschen nur Servernamen und Ports gegen die Daten Ihres Anbieters aus.

```bash
cat > ~/.config/himalaya/config.toml <<'EOF'
name = "ionos"
default = true

[accounts.ionos]
email = "ihre-adresse@example.com"

[accounts.ionos.imap]
host = "imap.ionos.de"
port = 993
encryption = "ssl"
login = "ihre-adresse@example.com"
passwd-cmd = "pass show ionos-mail"

[accounts.ionos.smtp]
host = "smtp.ionos.de"
port = 465
encryption = "ssl"
login = "ihre-adresse@example.com"
passwd-cmd = "pass show ionos-mail"
EOF
chmod 600 ~/.config/himalaya/config.toml
```

## 4. Passwort sicher hinterlegen

Speichern Sie das Mail-Passwort nicht direkt in der Konfigurationsdatei. Nutzen Sie einen Passwortspeicher oder eine vergleichbare sichere Methode. Wenn `pass` auf Ihrem Server noch nicht eingerichtet ist, richten Sie es zuerst ein oder bitten Sie OpenClaw um Hilfe:

```text
Hilf mir, auf meinem Server den Passwortspeicher `pass` einzurichten, damit Himalaya mein IONOS-Mail-Passwort sicher über `pass show ionos-mail` lesen kann. Erkläre jeden Schritt kurz, bevor du ihn ausführst.
```

Wenn `pass` bereits eingerichtet ist:

```bash
pass insert ionos-mail
```

## 5. Zugriff testen

```bash
himalaya account list
himalaya folder list
himalaya envelope list --folder INBOX --page-size 5
```

Wenn die letzten fünf Posteingangs-Mails erscheinen, ist die Verbindung bereit.

## 6. E-Mail-Triage in OpenClaw testen

Öffnen Sie OpenClaw und kopieren Sie diesen Prompt in den Chat:

```text
Prüfe mit Himalaya meine letzten 10 E-Mails im Posteingang. Zeige mir nur Nachrichten, auf die ich heute wahrscheinlich reagieren sollte. Ignoriere Newsletter, Systemmails, Werbung und offensichtliche automatische Benachrichtigungen.
```

## 7. HEARTBEAT.md ergänzen

Kopieren Sie diesen Text in OpenClaw, damit der Assistent die regelmäßige Mail-Prüfung in Ihre `HEARTBEAT.md` einträgt:

```text
Ergänze meine HEARTBEAT.md um eine regelmäßige E-Mail-Triage. Prüfe ungefähr alle 90 Minuten mit Himalaya den Posteingang. Melde mir nur wichtige neue Nachrichten, bei denen eine menschliche Reaktion sinnvoll ist. Ignoriere Newsletter, Werbung, Systembenachrichtigungen, noreply-Absender und bereits gemeldete Nachrichten.
```

## 8. Gateway neu starten

Damit OpenClaw die neue Heartbeat-Regel zuverlässig lädt, starten Sie den Gateway neu:

```bash
openclaw gateway restart
```

## Wenn etwas nicht klappt

Nutzen Sie diesen Prompt in OpenClaw und fügen Sie die Fehlermeldung darunter ein:

```text
Meine Himalaya-E-Mail-Einrichtung funktioniert noch nicht. Bitte lies die Fehlermeldung, finde die wahrscheinlichste Ursache und gib mir den nächsten sicheren Befehl. Ändere keine Passwörter und lösche keine Konfigurationsdateien ohne Rückfrage.
```
