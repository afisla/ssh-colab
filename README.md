<div align="center">

<img src="https://img.shields.io/badge/SSH-Tunnel-blue?style=for-the-badge&logo=terminal&logoColor=white" />
<img src="https://img.shields.io/badge/Google-Colab-F4B400?style=for-the-badge&logo=googlecolab&logoColor=white" />
<img src="https://img.shields.io/badge/Afisla-Tunnel-FF6B35?style=for-the-badge&logo=tunnel&logoColor=white" />

<br/>

# SSH COLAB

### Free SSH server on Google Colab with Afisla Tunnel

**No port forwarding. No public IP. Just run and connect.**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/afisla/ssh-colab/blob/main/ssh-colab-afisla.ipynb)

---

</div>

## Kodexplorer Notebook (SSH + File Manager)

[![Open Kodexplorer In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/afisla/ssh-colab/blob/main/Kodexplorer.ipynb)

`Kodexplorer.ipynb` menjalankan layanan gabungan:

- SSH Server di port `2222` melalui Afisla tunnel
- KODExplorer (web file manager) di port `8008` melalui Afisla tunnel domain `phpfile`

Saat tunnel aktif, output utama yang akan muncul:

```text
[AFISLA TUNNELS ACTIVE]
SSH User       : root
SSH Password   : <ROOT_PASSWORD>
SSH Relay Port : <RELAY_PORT>
KODExplorer URL: https://phpfile.afisla.web.id
```

Perintah SSH yang dihasilkan:

```bash
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=30 -o PreferredAuthentications=password -o ProxyCommand='nc relay.afisla.web.id <RELAY_PORT>' root@127.0.0.1 -p 2222
```

### Konfigurasi `Kodexplorer.ipynb`

Atur variabel berikut di awal cell:

```python
SSH_PORT = 2222
ROOT_PASSWORD = "Zavin123"
KODEXPLORER_PORT = 8008
KODEXPLORER_DIR = "/content/kodexplorer"
KODEXPLORER_DOMAIN = "phpfile"
```

### Dependensi yang dipasang otomatis (Kodexplorer)

- `openssh-server`, `curl`, `git`, `unzip`
- `php`, `php-cli`, `php-zip`, `php-mbstring`, `php-xml`, `php-curl`, `php-gd`
- Afisla tunnel client (`/usr/local/bin/afisla`)
- `opencode-ai` (via NVM + Node.js 24)

### Alur kerja notebook `Kodexplorer.ipynb`

1. Cleanup proses lama (`cloudflared`, `afisla`, `sshd`, `php`, dll)
2. Install dependensi sistem dan client Afisla
3. Clone `KODExplorer` ke `/content/kodexplorer`
4. Jalankan PHP built-in server di port `8008`
5. Setup dan jalankan SSHD di port `2222`
6. Jalankan 2 tunnel Afisla (SSH + KODExplorer)
7. Monitor proses dan tampilkan dashboard HTML + heartbeat

## What is this?

A single-click notebook that turns Google Colab into a **free SSH server** with public access via [Afisla TCP Relay](https://afisla.web.id).

Run it → get a tunnel → connect from anywhere.

## How it works

```
┌─────────────┐      ┌──────────────┐      ┌─────────────┐
│  Your PC    │ ───▶ │  Afisla      │ ───▶ │  Colab VM   │
│  (terminal) │      │  Relay       │      │  (SSH)      │
└─────────────┘      └──────────────┘      └─────────────┘
```

## Quick Start

### Step 1 — Open the notebook

Click the badge above or [open directly](https://colab.research.google.com/github/afisla/ssh-colab/blob/main/ssh-colab-afisla.ipynb).

### Step 2 — Run all cells

`Runtime` → `Run all` or press `Ctrl+F9`

### Step 3 — Copy the SSH command

When ready, the notebook outputs:

```
[AFISLA TUNNEL READY]
User     : root
Password : Zavin123
Port Relay: 12345

Copy & run this in your local terminal:
ssh -o ServerAliveInterval=30 -o PreferredAuthentications=password -o ProxyCommand='nc relay.afisla.web.id 12345' root@127.0.0.1 -p 2222
```

### Step 4 — Connect

Paste the command in your **local terminal** (not Colab). Done.

## What gets installed

| Package | Purpose |
|---------|---------|
| `openssh-server` | SSH daemon |
| `netcat-openbsd` | TCP relay connection (`nc` proxy) |
| `curl` | Download Afisla client |
| `afisla` | Tunnel client ([afisla.web.id](https://afisla.web.id)) |

All installed automatically on first run.

## What the notebook does

```
1. Cleanup     → Kill old sessions (sshd, cloudflared, afisla, etc.)
2. Install     → apt-get update + install missing packages + Afisla client
3. SSH Setup   → Generate host keys, start sshd on port 2222
4. Tunnel      → Connect to Afisla relay for public access
5. Monitor     → Auto-restart sshd if crashed, detect relay port
```

## Configuration

Edit these in the first cell:

```python
SSH_PORT = 2222          # SSH server port
ROOT_PASSWORD = "Zavin123"  # SSH password
```

That's it. Login is always **root** with password set by Colab.

## Requirements

- Google account (for Colab)
- `ssh` client on your local machine
- `nc` (netcat) on your local machine for the proxy command

### Install netcat locally

<details>
<summary><b>macOS</b></summary>

```bash
brew install netcat
```

</details>

<details>
<summary><b>Ubuntu / Debian</b></summary>

```bash
sudo apt install netcat-openbsd
```

</details>

<details>
<summary><b>Windows (PowerShell)</b></summary>

Netcat is available via WSL or you can use `plink` from PuTTY.

```bash
# WSL
sudo apt install netcat-openbsd
```

</details>

## Troubleshooting

| Issue | Fix |
|-------|-----|
| No relay port shown | Wait 15-30s for Afisla to connect |
| `Connection refused` | Re-run all cells |
| Colab session dies | Re-run — cleanup handles stale processes |
| `nc: command not found` | Install netcat locally (see above) |

---

<div align="center">

**Built by [afisla](https://github.com/afisla)**

</div>
