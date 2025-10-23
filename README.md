# 🏔️ Rainier Server

[![Status](https://img.shields.io/badge/status-live-success?style=flat-square)](https://rainierserver.com)
[![Host](https://img.shields.io/badge/host-HP%20Z440-lightgrey?style=flat-square)](#)
[![Built with](https://img.shields.io/badge/built%20with-Nginx%20%7C%20Cloudflare%20Tunnels%20%7C%20Ubuntu-blue?style=flat-square)](#)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)

> **Rainier Server** is my personal homelab and development environment — a hybrid workstation and playground for web, AI/ML, and creative projects.  
> This repository contains the public static site served at **[rainierserver.com](https://rainierserver.com)** via a self-hosted Nginx instance behind a Cloudflare Tunnel.

---

## 🌐 Live Site
**🔗 [https://rainierserver.com](https://rainierserver.com)**  
Hosted on my HP Z440 workstation “Rainier,” running Ubuntu 24.04 LTS.

![Rainier Background](https://rainierserver.com/assets/rainier-sunset.jpg)

---

## 🧭 Overview

Rainier is where I bring together my passions for **software engineering**, **AI/ML experimentation**, and **systems design**.  
It’s powered by robust enterprise hardware and secured with a fully private backend accessible via Tailscale.

### ⚙️ Core Stack

| Component | Details |
|------------|----------|
| **Hardware** | HP Z440 Workstation – Xeon E5-2690 v4 (14C/28T), 64 GB DDR4 ECC RAM |
| **GPUs** | NVIDIA RTX A4000 + Quadro P2000 |
| **Storage** | 2× 2 TB NVMe (mirror) + 3× 12 TB Exos X18 (RAIDZ1) |
| **OS** | Ubuntu 24.04 LTS (Noble Numbat) |
| **Network** | Intel i350 Dual GbE + Tailscale |
| **Web Server** | Nginx (127.0.0.1:80) served via Cloudflare Tunnel |
| **Domain** | [rainierserver.com](https://rainierserver.com) (CNAME through Cloudflare) |

---

## 💡 Features

- 🧠 **AI / ML Playground** – GPU-accelerated PyTorch environment for model training, inference, and experimentation.  
- 🧩 **Web Sandbox** – Node.js + Python stack for quick prototyping and local dev.  
- 📊 **Monitoring Stack** – Grafana + Prometheus for real-time system metrics.  
- 🎮 **Minecraft Server** – Because even engineers need fun downtime.  
- 📺 **Plex Media Server** – Self-hosted streaming for personal archives.  

---

## 🔒 Infrastructure Diagram

Nginx (localhost:80)
├── /var/www/rainier → Static site
├── /assets/ → /mnt/Helicon/about.rainier
↓
Cloudflared Tunnel (Systemd Service)
↓
Cloudflare DNS (rainierserver.com, www)

yaml
Copy code

---

## 🧰 Quick Reference

### 🔧 Nginx Site Configuration
```nginx
server {
    listen 127.0.0.1:80 default_server;
    server_name rainierserver.com www.rainierserver.com;

    root /var/www/rainier;
    index index.html;

    location /assets/ {
        alias /mnt/Helicon/about.rainier/;
        expires 30d;
        add_header Cache-Control "public, max-age=2592000";
    }

    location / {
        try_files $uri $uri/ =404;
    }
}
```

🛡️ Security Headers
Copy code
```
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
```

🚀 Deploy Workflow
```bash
Copy code
# Pull latest changes from GitHub
cd /var/www/rainier
git pull origin main

# Validate and reload Nginx
sudo nginx -t && sudo systemctl reload nginx

# Confirm tunnel and site health
systemctl status cloudflared
curl -I -H "Host: rainierserver.com" http://127.0.0.1/
```

🧑‍💻 Author
Forrest Morrisey
🌐 forrestmorrisey.com
💼 LinkedIn | 🐙 GitHub

📸 Credits
Background photo: Caleb Ralston – Unsplash

Hosted on Rainier 🖥️ — Ubuntu 24.04 + Nginx + Cloudflare Tunnel

© 2025 Forrest Morrisey. All rights reserved.