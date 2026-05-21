# Kapitel 3: Server mieten und absichern

## SSH Verbindung

```bash
ssh root@DEINE_IP_ADRESSE
```

## Tailscale installieren

```bash
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up --ssh
# Link im Browser öffnen und verbinden
tailscale status
# Tailscale-IP merken (100.x.x.x)
```

## Firewall einrichten

```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow 41641/udp
ufw allow in on tailscale0
# Test: ssh root@100.x.x.x (sollte funktionieren)
ufw enable
# Mit 'y' bestätigen
```

## SSH Härtung (optional)

### 1. Auf Server
```bash
adduser DEIN_NAME
usermod -aG sudo DEIN_NAME
```

### 2. Auf lokalem Rechner
```bash
ssh-keygen -t ed25519 -C "DEIN_NAME@laptop"
# 3x Enter drücken
cat ~/.ssh/id_ed25519.pub
# Key kopieren und in Hetzner Dashboard hinterlegen
```

### 3. Key auf Server kopieren
```bash
ssh-copy-id DEIN_NAME@DEINE_IP_ADRESSE
```

### 4. SSH-Konfiguration ändern
```bash
sudo nano /etc/ssh/sshd_config
```

Ändern/ergänzen:
```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Speichern: `Strg+O` → Enter → `Strg+X`

### 5. SSH neu starten
```bash
sudo systemctl restart ssh
```

---

**Erklärungen im Buch.**  
Updates auf: https://berlow.de/openclaw-material