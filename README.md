
# 🏠 Home Server Setup

*A container-based personal cloud powered by Ubuntu Server, CasaOS, Nextcloud, Immich, AdGuard, Tailscale, and SSH*

## 📘 Introduction

This project documents the setup of a secure and extensible home server using **Ubuntu Server**, managed with **CasaOS**, and extended with services such as **Nextcloud**, **Immich**, **AdGuard Home**, and **Tailscale** for secure remote access.
SSH is used for administration, and all services run in **Docker containers**.

---

## 📑 Table of Contents

* [Features](#-features)
* [Hardware](#-hardware)
* [Architecture Overview](#-architecture-overview)
* [Installation](#-installation)
* [Usage](#-usage)
* [Service Configuration](#-service-configuration)
* [Tailscale Remote Access](#-tailscale-remote-access)
* [Dependencies](#-dependencies)
* [Examples](#-examples)
* [Troubleshooting](#-troubleshooting)
* [Contributors](#-contributors)
* [License](#-license)

---

## ✨ Features

* CasaOS dashboard for managing containers
* Nextcloud for personal cloud storage
* Immich for photo and video backup
* AdGuard Home for network-wide ad blocking
* Tailscale for secure remote access using WireGuard
* SSH access for server control
* Fully containerized deployment
* Local or remote access through Tailscale

---

## 🖥 Hardware

**Lenovo ThinkCentre M720s**

* CPU: Intel Core i5 (8th Gen)
* RAM: 8 GB
* Storage: 2 TB HDD
* OS: Ubuntu Server LTS
* Network: Gigabit Ethernet

---

## 🏗 Architecture Overview

```
                             Ubuntu Server
                     ┌──────────────┬──────────────┐
                     │              │              │
                   SSH         Tailscale VPN   Docker Engine
                                                  │
       ┌───────────────────┬───────────────┬───────────────┬────────────────┐
       │                   │               │                │                │
    CasaOS            Nextcloud         Immich        AdGuard Home      + more
```

---

## 🛠 Installation

### 1. Install Ubuntu Server

Update the system:

```bash
sudo apt update && sudo apt upgrade -y
```

Enable OpenSSH Server during installation or run:

```bash
sudo apt install openssh-server
```

---

### 2. Enable Secure SSH

Check SSH status:

```bash
sudo systemctl status ssh
```

(Optional) Change SSH port:

```
sudo nano /etc/ssh/sshd_config
```

Recommended changes:

```
Port 2222
PasswordAuthentication no
PermitRootLogin no
```

Reload:

```bash
sudo systemctl reload ssh
```

---

### 3. Install Docker & Docker Compose

```bash
sudo apt install docker.io docker-compose -y
sudo systemctl enable --now docker
```

---

### 4. Install CasaOS

```bash
curl -fsSL https://get.casaos.io | sudo bash
```

Access at:
**http://<server-ip>**

---

### 5. Install Tailscale

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

For enhanced features:

```bash
sudo tailscale up --ssh --accept-dns --accept-routes
```

---

### 6. Install Apps (via CasaOS or Docker Compose)

**Nextcloud example:**

```bash
docker run -d --name nextcloud -p 8080:80 -v nc_data:/var/www/html nextcloud
```

**AdGuard Home example:**

```bash
docker run -d --name adguardhome -p 53:53/tcp -p 53:53/udp -p 3000:3000 adguard/adguardhome
```

Immich is best installed using a full docker-compose bundle.

---

## 🚀 Usage

### Accessing Services

| Service      | URL                     |
| ------------ | ----------------------- |
| CasaOS       | http://<server-ip>      |
| Nextcloud    | http://<server-ip>:8080 |
| Immich       | http://<server-ip>:2283 |
| AdGuard Home | http://<server-ip>:3000 |

### SSH Access

```bash
ssh <user>@<server-ip>
```

### SSH via Tailscale (remote)

```bash
ssh <user>@<tailscale-ip>
```

---

## 🔐 Tailscale Remote Access

Tailscale enables:

* Encrypted remote access
* No port forwarding
* MagicDNS support
* Built-in SSH authentication

Connect using:

```bash
ssh <user>@servername.tailnet-name.ts.net
```

---

## ⚙ Service Configuration

### Nextcloud

* Store user data in a Docker volume
* Add apps for contacts, calendar, notes
* Optional: add Redis caching for performance

### Immich

* Ideal for mobile photo backups
* Connect mobile app to server via Tailscale or LAN

### AdGuard Home

* Set your router’s DNS to the server IP
* Add recommended blocklists like OISD

---

## 📦 Dependencies

* Ubuntu Server LTS
* Docker + Docker Compose
* CasaOS
* Tailscale
* Nextcloud container
* Immich container
* AdGuard Home container

---

## 📘 Examples

### Restart all containers:

```bash
docker compose down && docker compose up -d
```

### Backup volumes:

```bash
sudo tar -czvf backups.tar.gz /var/lib/docker/volumes
```

### Check Tailscale connection:

```bash
tailscale status
```

---

## 🧰 Troubleshooting

| Issue                    | Solution                                                |
| ------------------------ | ------------------------------------------------------- |
| Can't SSH remotely       | Ensure Tailscale is connected & firewall allows traffic |
| Slow Nextcloud           | Move to SSD or add caching                              |
| AdGuard not blocking ads | Router DNS not updated                                  |
| Immich upload errors     | Check RAM usage or database container                   |

---

## 👥 Contributors

Ken
---

## 📄 License
No licenses

