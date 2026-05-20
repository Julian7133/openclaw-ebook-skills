# Kapitel 3: Server mieten und absichern

Copy-Paste-Blatt für die Server-Basis. Ersetzen Sie Platzhalter wie `123.123.123.123`, `100.x.x.x` und `DEIN_NAME` durch Ihre eigenen Werte.

## SSH-Verbindung

```bash
ssh root@123.123.123.123
```

Erfolgsindikator auf dem Server:

```text
root@openclaw-prod:~#
```

## Tailscale installieren

Auf dem Server ausführen:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up --ssh
```

Danach den angezeigten Login-Link im Browser öffnen und den Server mit dem Tailnet verbinden.

Status prüfen:

```bash
tailscale status
```

## Firewall mit ufw

Vor dem Aktivieren sicherstellen, dass `tailscale status` den Server als online zeigt.

```bash
# Sicherer Standard: alles eingehend blockieren
ufw default deny incoming
ufw default allow outgoing

# Tailscale WireGuard-Port für direkte Peer-to-Peer-Verbindungen
ufw allow 41641/udp

# Gesamten Tailscale-Verkehr erlauben, inklusive SSH via Tailnet
ufw allow in on tailscale0

# Firewall aktivieren
ufw enable
```

Mit `y` bestätigen.

Zweites Terminal öffnen und SSH über die Tailscale-IP testen:

```bash
ssh root@100.x.x.x
```

Firewall-Status prüfen:

```bash
ufw status verbose
```

## SSH-Härtung (optional, empfohlen)

Vorher im Hetzner-Dashboard einen Snapshot anlegen.

Neuen Benutzer auf dem Server anlegen:

```bash
adduser DEIN_NAME
usermod -aG sudo DEIN_NAME
```

SSH-Schlüssel lokal erzeugen:

```bash
ssh-keygen -t ed25519 -C "DEIN_NAME@laptop"
cat ~/.ssh/id_ed25519.pub
```

Öffentlichen Schlüssel in Hetzner hinterlegen oder auf den Server kopieren:

```bash
ssh-copy-id DEIN_NAME@123.123.123.123
ssh DEIN_NAME@123.123.123.123
```

SSH-Konfiguration auf dem Server bearbeiten:

```bash
sudo nano /etc/ssh/sshd_config
```

Diese Zeilen müssen ohne führende Raute vorhanden sein:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Speichern: `Strg+O` → Enter → `Strg+X`

SSH neu starten:

```bash
sudo systemctl restart ssh
```

Sofort in einem neuen lokalen Terminal testen:

```bash
ssh DEIN_NAME@123.123.123.123
```

## Kapitel-Check

- Hetzner-Server: CX23, Ubuntu 24.04 LTS, Standort Nürnberg oder Falkenstein
- SSH-Zugang funktioniert
- Tailscale verbunden
- `ufw status verbose` zeigt die erwarteten Regeln
- Optional: Root-Login deaktiviert und sudo-Benutzer getestet

---

**Erklärungen im Buch.**  
Updates auf: https://berlow.de/openclaw-guides
