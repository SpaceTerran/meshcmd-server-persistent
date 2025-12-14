# meshcmd-server-persistent

Dockerized **MeshCommander** with **server-side persistence** for the computer list, designed for Intel® AMT / vPro environments.

> ⚠️ This is an **unofficial**, community-maintained project.  
> It is not affiliated with or endorsed by Intel or the MeshCommander upstream project.

---

## 📖 Full Documentation

**For complete setup instructions, deployment guide, and detailed explanations, see the blog post:**

👉 **[A Better Way to Run MeshCommander in Your Homelab (Intel vPro + Docker)](https://spaceterran.com/posts/meshcommander-docker-persistent-amt-management/)**

The blog post covers:
- Step-by-step setup instructions
- Docker Compose configuration
- Traefik integration
- Security considerations
- Troubleshooting tips

---

## Quick Overview

MeshCommander traditionally stores its computer list in browser `localStorage`, which means entries are lost when:
- Browser cache is cleared
- A different browser is used
- You access it from another machine

This project packages MeshCommander into a Docker container and applies build-time patches that allow the computer list to be stored **server-side** on a persistent volume.

## Features

- Server-side persistent computer list
- Works across browsers and devices
- Docker-based deployment
- Optional Traefik / reverse-proxy support
- Node.js 16 for compatibility

## Security Notice

> **Passwords may be stored in plain text.**

If you choose to save credentials, they are written unencrypted to `data/computerlist.config`.

**Recommendations:**
- Do not save passwords if you are uncomfortable with this
- Restrict filesystem permissions on the data directory
- Run only on trusted hosts and networks
- Prefer HTTPS when exposing the service

## Upstream & Attribution

This project is based on the following upstream work:

- **MeshCommander** by Ylian Saint-Hilaire  
  https://github.com/Ylianst/MeshCommander

- **meshcmd Docker image** by brytonsalisbury  
  https://hub.docker.com/r/brytonsalisbury/meshcmd

## License

MIT License.
