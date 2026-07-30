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

## Troubleshooting

| Issue | Fix |
|-------|-----|
| No relay port shown | Wait 15-30s for Afisla to connect |
| `Connection refused` | Re-run all cells |
| Colab session dies | Re-run — cleanup handles stale processes |

---

<div align="center">

**Built by [afisla](https://github.com/afisla)**

</div>
