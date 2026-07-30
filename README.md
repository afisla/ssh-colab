<div align="center">

# SSH-COLAB

### SSH Tunneling via Google Colab with Cloudflare

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/afisla/ssh-colab/blob/main/ssh-colab-afisla.ipynb)

---

</div>

## About

Run a full SSH server directly from **Google Colab** for free. This notebook sets up:

- **OpenSSH server** on a custom port
- **Cloudflare Tunnel** for public access (no port forwarding needed)
- **Auto-cleanup** of old sessions and processes
- **User management** with configurable credentials

## Quick Start

1. Click the **Open in Colab** badge above
2. Set your `USERNAME` and `PASSWORD` in the configuration cell
3. Run all cells
4. Use the generated Cloudflare tunnel URL to connect via SSH

```bash
ssh -o StrictHostKeyChecking=no -p 2222 your_username@your_tunnel.trycloudflare.com
```

## Features

| Feature | Description |
|---------|-------------|
| Free SSH Server | Runs entirely on Google Colab's free GPU runtime |
| Cloudflare Tunnel | No port forwarding or public IP required |
| Custom Port | SSH on port `2222` (configurable) |
| Session Cleanup | Automatically removes old sessions and logs |
| Persistent Auth | Maintains connection across Colab sessions |

## Requirements

- Google account (for Colab)
- Nothing else needed (dependencies are auto-installed)

## Disclaimer

This project is for **educational purposes only**. Use responsibly and in compliance with Google Colab's Terms of Service.

---

<div align="center">

Made with by [afisla](https://github.com/afisla)

</div>
