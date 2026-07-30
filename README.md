<div align="center">

# SSH-COLAB

### Free SSH Tunneling via Google Colab with Afisla Tunnel

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/afisla/ssh-colab/blob/main/ssh-colab-afisla.ipynb)

</div>

---

## What This Does

Run a persistent **SSH server** on **Google Colab** for free, with public access via **Afisla TCP Relay Tunnel**. No port forwarding, no public IP needed.

## How It Works

1. **Cleanup** — Kills any old sessions (cloudflared, sshd, afisla, etc.) and removes leftover files
2. **Install Dependencies** — Auto-installs `openssh-server`, `netcat-openbsd`, `curl`, and the **Afisla Tunnel Client**
3. **User Setup** — Creates a Linux user with your chosen credentials (also sets root password)
4. **SSH Server** — Starts OpenSSH on port `2222` with password auth enabled
5. **Afisla Tunnel** — Connects to Afisla relay for public SSH access
6. **Monitor** — Background thread auto-restarts SSHD if it dies and prints the connection command when relay port is detected

## Configuration

Edit these variables in the first cell:

```python
USERNAME = "zavin"       # Your SSH username
PASSWORD = "Zavin123"    # Your SSH password
SSH_PORT = 2222          # SSH server port
```

## Connect

After running, the notebook outputs a connection command like:

```bash
ssh -o ServerAliveInterval=30 \
    -o PreferredAuthentications=password \
    -o ProxyCommand='nc relay.afisla.web.id <RELAY_PORT>' \
    zavin@127.0.0.1 -p 2222
```

Copy and run it in your **local terminal**.

## Features

| Feature | Detail |
|---------|--------|
| Free | Runs on Google Colab free tier |
| Auto-Install | Installs all dependencies automatically |
| Auto-Cleanup | Kills old sessions before starting |
| Auto-Restart | Background monitor restarts SSHD if crashed |
| Custom User | Create your own username/password |
| Relay Tunnel | Public access via Afisla TCP relay |
| Verbose Logging | SSH set to `LogLevel VERBOSE` |

## Requirements

- Google account (for Colab)
- That's it — everything else is auto-installed

## Troubleshooting

| Problem | Solution |
|---------|----------|
| No relay port shown | Wait 15-30 seconds for Afisla to connect |
| SSH connection refused | Re-run all cells to restart services |
| Colab disconnects | Re-run — session cleanup handles stale processes |

---

<div align="center">

Built by [afisla](https://github.com/afisla)

</div>
