![Docker](https://img.shields.io/badge/docker-ready-blue) ![Contributions](https://img.shields.io/badge/contributions-welcome-orange) ![Last Commit](https://img.shields.io/github/last-commit/Bme-Adib/self-host-ai-advanced-kit) ![Version](https://img.shields.io/badge/version-1.0.0-green)




# Self-hosted AI Starter Kit

Easily host your own AI and low-code development environment with Docker. This template combines **n8n**, **Ollama**, **Open WebUI**, **Qdrant**, and **PostgreSQL** into one self-hosted stack.

![n8n](https://img.shields.io/badge/n8n-EA4C89?logo=n8n&logoColor=white) ![Ollama](https://img.shields.io/badge/ollama-%23000000.svg?style=for-the-badge&logo=ollama&logoColor=white) ![Postgres](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white) ![Qdrant](https://img.shields.io/badge/Qdrant-FF4B4B?logo=qdrant&logoColor=white)

Curated by [n8n.io](https://github.com/n8n-io), forked and customized here to include:

* 🎶 **Domain support** (connect Google services securely)
* 🎶 **Step-by-step video tutorial**
* 🎶 **Open WebUI accessible remotely**



---

## Table of Contents

* [What’s Included](#whats-included)
* [What You Can Build](#what-you-can-build)
* [Prerequisites](#prerequisites)
* [Step 1: Install Docker](#step-1-install-docker)
* [Step 2: Install Docker Engine & Compose](#step-2-install-docker-engine--compose)
* [Step 3: Clone the Repository](#step-3-clone-the-repository)
* [Step 4: Run the Stack](#step-4-run-the-stack)

  * [Nvidia GPU](#for-nvidia-gpu-users)
  * [AMD GPU](#for-amd-gpu-users-on-linux)
  * [CPU Only](#to-run-using-cpu-only)
* [Upgrading](#upgrading)
* [Cloudflare Tunnel on Windows](#cloudflare-tunnel-on-windows)
* [Contact](#contact)

---

## What’s Included

✅ **[n8n](https://n8n.io/):** Low-code automation platform with AI components
✅ **[Ollama](https://ollama.com/):** Cross-platform LLM runner
✅ **[Open WebUI](https://openwebui.com/):** Self-hosted AI chat interface
✅ **[Qdrant](https://qdrant.tech/):** High-performance vector database
✅ **[PostgreSQL](https://www.postgresql.org/):** Reliable SQL database engine

---

## What You Can Build

⭐ AI Agents for scheduling and automation
⭐ Secure PDF summarization without data leaks
⭐ Smarter Slack/Discord bots
⭐ Private financial document analysis

---

## Prerequisites

* Ubuntu 20.04 or later
* A user account with **sudo** privileges
* Basic familiarity with the terminal

---
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
## Step 1: Install Docker

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu   $(. /etc/os-release && echo \"${UBUNTU_CODENAME:-$VERSION_CODENAME}\") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

---

## Step 2: Install Docker Engine & Compose

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## Step 3: Clone the Repository

```bash
git clone https://github.com/Bme-Adib/self-host-ai-advanced-kit.git
cd self-host-ai-advanced-kit
```

---

## Step 4: Run the Stack

### For Nvidia GPU Users

```bash
docker compose --profile gpu-nvidia up -d
```

> **Note:** If you haven’t used Nvidia GPU with Docker before, follow [Ollama GPU setup guide](https://github.com/ollama/ollama/blob/main/docs/docker.md).

### For AMD GPU Users on Linux

```bash
docker compose --profile gpu-amd up -d
```

### To Run Using CPU Only

```bash
docker compose up -d
```

---

## Upgrading

From the project folder:

```powershell
docker compose down
docker compose pull
```

Then restart using your preferred profile (`gpu-nvidia`, `gpu-amd`, or CPU only).

---

# Cloudflare Tunnel on Windows

You can expose your services securely using Cloudflare Tunnels on Windows. We will use **cloudflared** with **nssm.exe** (Non-Sucking Service Manager) to run it as a service.

### 1. Download & Install `cloudflared`

* Download from [Cloudflare releases](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation/).
* Place `cloudflared.exe` in `C:\cloudflared`.

### 2. Authenticate with Cloudflare

```powershell
cloudflared tunnel login
```

* A browser window opens. Log in and select your domain.
* A certificate is saved in `C:\Users\\<YourUser>\\.cloudflared`.

### 3. Create a Tunnel

```powershell
cloudflared tunnel create my-tunnel
```

* Replace `my-tunnel` with a custom name.
* Save the **Tunnel ID**.

### 4. Create a Config File

Create `C:\cloudflared\config.yml`:

```yaml
tunnel: <TUNNEL-ID>
credentials-file: C:\\Users\\<YourUser>\\.cloudflared\\<TUNNEL-ID>.json

ingress:
  - hostname: mydomain.com
    service: http://localhost:3000
  - service: http_status:404
```

### 5. Route Domain

```powershell
cloudflared tunnel route dns my-tunnel mydomain.com
```

### 6. Install `nssm.exe`

* Download from [nssm.cc](https://nssm.cc/download).
* Place `nssm.exe` in `C:\nssm`.

### 7. Register Cloudflare Tunnel as a Service

```powershell
nssm install CloudflareTunnel "C:\\cloudflared\\cloudflared.exe" "tunnel run my-tunnel"
```

Then start the service:

```powershell
nssm start CloudflareTunnel
```

---

## ✅ Done!

Your self-hosted AI stack is now accessible via:

```
https://mydomain.com
```

---

## Contact

Created by **The Biomed Nest**

* 📺 [YouTube Channel](https://www.youtube.com/@TheBiomedNest)
* 📸 [Instagram](https://www.instagram.com/thebiomednest)

Feel free to reach out with questions, suggestions, or feedback!
