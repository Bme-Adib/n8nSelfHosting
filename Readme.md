![Docker](https://img.shields.io/badge/docker-ready-blue) ![Contributions](https://img.shields.io/badge/contributions-welcome-orange) ![Last Commit](https://img.shields.io/github/last-commit/Bme-Adib/n8nSelfHosting) ![Version](https://img.shields.io/badge/version-2.0.0-green)

# n8n Self Hosting

Easily host your own [n8n](https://n8n.io/) automation platform using Docker on Ubuntu.
This guide is written step-by-step for beginners.

---

## Table of Contents

* [Prerequisites](#prerequisites)
* [Step 1: Install Docker](#step-1-install-docker)
* [Step 2: Install Docker Engine & Compose](#step-2-install-docker-engine--compose)
* [Step 3: Clone the Repository](#step-3-clone-the-repository)
* [Step 4: Start n8n](#step-4-start-n8n)
* [Access n8n](#access-n8n)
* [Cloudflare Tunnel Setup](#cloudflare-tunnel-setup-on-ubuntu-2404-lts)
* [Notes](#notes)
* [Contact](#contact)

---

## Prerequisites

* Ubuntu 20.04 or later
* A user account with **sudo** privileges
* Basic familiarity with the terminal

---

## Step 1: Install Docker

Run the following commands to install Docker’s official GPG key and repository:

```bash
# Update package index
sudo apt-get update

# Install required packages
sudo apt-get install -y ca-certificates curl

# Add Docker’s official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker repository to Apt sources
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu   $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package index again
sudo apt-get update
```

---

## Step 2: Install Docker Engine & Compose

Now install the latest version of Docker and Docker Compose:

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## Step 3: Clone the Repository

Download the repository and move into the project folder:

```bash
cd ~
git clone https://github.com/Bme-Adib/n8nSelfHosting.git
cd n8nSelfHosting
```

---

## Step 4: Start n8n

Start the container in detached mode:

```bash
docker compose up -d
```

---

## Access n8n

Once running, you can access your self-hosted n8n instance at:

```
http://YOUR_SERVER_IP:5678
```

> **Note:** Google products (like Gmail or Google Sheets integrations) require a valid domain name. They will not work with just an IP address.

---

# Cloudflare Tunnel Setup on Ubuntu 24.04 LTS

This guide explains how to install and configure **cloudflared** to expose your local services securely to the internet using **Cloudflare Tunnels**.

---

## 1. Install `cloudflared`

```bash
# Add Cloudflare GPG key
sudo mkdir -p --mode=0755 /usr/share/keyrings
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | \
  sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null

# Add repository
echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] \
https://pkg.cloudflare.com/cloudflared $(lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/cloudflared.list

# Update and install
sudo apt update && sudo apt install cloudflared -y
```

---

## 2. Authenticate with Cloudflare

```bash
cloudflared tunnel login
```

* A browser window will open.
* Log in with your Cloudflare account and select the domain.
* A certificate will be saved in `~/.cloudflared`.

---

## 3. Create a Tunnel

```bash
cloudflared tunnel create my-tunnel
```

* Replace `my-tunnel` with your preferred tunnel name.
* Note the **Tunnel ID** that gets generated.

---

## 4. Configure the Tunnel

Create a configuration file:

```bash
sudo nano /etc/cloudflared/config.yml
```

Example:

```yaml
tunnel: <TUNNEL-ID>
credentials-file: /root/.cloudflared/<TUNNEL-ID>.json

ingress:
  - hostname: mydomain.com
    service: http://localhost:8080
  - service: http_status:404
```

Replace:

* `<TUNNEL-ID>` with your actual tunnel ID.
* `mydomain.com` with your real domain.
* `8080` with the port of your service.

---

## 5. Route Domain to Tunnel

```bash
cloudflared tunnel route dns my-tunnel mydomain.com
```

This creates a DNS record in Cloudflare that points `mydomain.com` to the tunnel.

---


## 6. Enable Tunnel as a Service

```bash
sudo cloudflared service install
```

This ensures the tunnel starts automatically on boot.

---

## ✅ Done!

Your service should now be accessible at:

```
https://mydomain.com
```

---

## Notes

* You can add multiple subdomains by extending the `ingress` section in `config.yml`.
* Example for multiple services:

```yaml
ingress:
  - hostname: chat.mydomain.com
    service: http://localhost:3000
  - hostname: app.mydomain.com
    service: http://localhost:5000
  - service: http_status:404
```

---

## Contact

Created by **The Biomed Nest**

* 📺 [YouTube Channel](https://www.youtube.com/@TheBiomedNest)
* 📸 [Instagram](https://www.instagram.com/thebiomednest)

Feel free to reach out with questions, suggestions, or feedback!
