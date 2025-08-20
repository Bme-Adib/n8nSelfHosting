# n8n Self Hosting  

Easily host your own [n8n](https://n8n.io/) automation platform using Docker on Ubuntu.  
This guide is written step-by-step for beginners.  

![Docker](https://img.shields.io/badge/docker-ready-blue)   ![Contributions](https://img.shields.io/badge/contributions-welcome-orange)  

---

## Table of Contents  
- [Prerequisites](#prerequisites)  
- [Step 1: Install Docker](#step-1-install-docker)  
- [Step 2: Install Docker Engine & Compose](#step-2-install-docker-engine--compose)  
- [Step 3: Clone the Repository](#step-3-clone-the-repository)  
- [Step 4: Start n8n](#step-4-start-n8n)  
- [Access n8n](#access-n8n)  
- [Contact](#contact)  
- [License](#license)  

---

## Prerequisites  

- Ubuntu 20.04 or later  
- A user account with **sudo** privileges  
- Basic familiarity with the terminal  

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

## Contact  

Created by **The Biomed Nest**  

- 📺 [YouTube Channel](https://www.youtube.com/@TheBiomedNest)  
- 📸 [Instagram](https://www.instagram.com/thebiomednest)  

Feel free to reach out with questions, suggestions, or feedback!  

---

## License  

This project is licensed under the MIT License.  
See the [LICENSE](LICENSE) file for details.  
