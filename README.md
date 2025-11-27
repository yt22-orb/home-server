# home-server

🏠 Home Server Setup

A container-based personal cloud powered by Ubuntu Server, CasaOS, Nextcloud, Immich, AdGuard, Tailscale, and SSH

📘 Introduction

This project sets up a secure and extensible home server using Ubuntu Server, managed with CasaOS, and enhanced with private cloud tools such as Nextcloud, Immich, and AdGuard Home.
Remote access is enabled using Tailscale, and administrative control is available through secure SSH.

📑 Table of Contents

Features

Hardware

Architecture Overview

Installation

Ubuntu Server

SSH Setup

Docker

CasaOS

Tailscale

Application Stack

Usage

Service Configuration

Tailscale Remote Access

Dependencies

Examples

Troubleshooting

Contributors

License

✨ Features

CasaOS Dashboard – User-friendly UI for managing containers and storage.

Nextcloud – Personal cloud for files, contacts, notes, and sync.

Immich – High-performance photo and video backup platform.

AdGuard Home – Network-wide ad & tracker blocking.

Tailscale – Zero-config secure remote access over WireGuard.

SSH – Direct secure shell access for system maintenance.

Containerized Architecture – Simple updates, rollbacks, and backups.

Local or remote access – Safe connections from anywhere using Tailscale.

🖥 Hardware

Lenovo ThinkCentre M720s

CPU: Intel Core i5 (8th Gen)

RAM: 8 GB

Storage: 2 TB HDD

OS: Ubuntu Server (LTS recommended)

Network: Gigabit Ethernet

🏗 Architecture Overview
                             ┌───────────────────────────┐
                             │       Ubuntu Server        │
                             └───────┬─────────┬─────────┘
                                     │         │
                             Secure SSH      Tailscale VPN
                                     │         │
                           Container Engine (Docker)
                                     │
     ┌───────────────────┬─────────────────────┬─────────────────────┐
     │                   │                     │                     │
┌──────────────┐   ┌───────────────┐   ┌────────────────┐    ┌─────────────────┐
│   CasaOS     │   │   Nextcloud   │   │     Immich      │    │  AdGuard Home   │
└──────────────┘   └───────────────┘   └────────────────┘    └─────────────────┘

🛠 Installation
1. Install Ubuntu Server

Download and install Ubuntu Server LTS.

Enable OpenSSH Server during installation.

Update packages:

sudo apt update && sudo apt upgrade -y

2. Enable Secure SSH

SSH is essential for remote administration.

Check SSH status:
sudo systemctl status ssh

(Optional) Change default SSH port:
sudo nano /etc/ssh/sshd_config


Edit:

Port 2222
PasswordAuthentication no


Then reload:

sudo systemctl reload ssh

Recommended Security:

Disable root login

Use SSH keys only

Allow LAN + Tailscale traffic only via firewall
(Ask if you want these rules auto-generated.)

3. Install Docker & Docker Compose
sudo apt install docker.io docker-compose -y
sudo systemctl enable --now docker

4. Install CasaOS
curl -fsSL https://get.casaos.io | sudo bash


Open UI:
http://<server-ip>

5. Install Tailscale

Tailscale enables secure remote access without port forwarding.

curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up

Optional recommended flags:
sudo tailscale up --ssh --accept-routes --accept-dns


--ssh: allows remote SSH via Tailscale identity

--accept-routes: use subnet routing if configured

--accept-dns: allow MagicDNS

After setup, you can reach your server remotely using:

ssh ubuntu@<TAILSCALE-IP>

6. Install Applications

Services installed via CasaOS or Docker Compose:

Nextcloud

Immich

AdGuard Home

Example Nextcloud command:

docker run -d --name nextcloud -p 8080:80 -v nc_data:/var/www/html nextcloud

🚀 Usage
Accessing Services (Local or via Tailscale IP)
Service	URL
CasaOS	http://<server-ip>
Nextcloud	http://<server-ip>:8080
Immich	http://<server-ip>:2283
AdGuard Home	http://<server-ip>:3000
SSH Access
ssh <username>@<server-ip>

Remote SSH via Tailscale
ssh <username>@<tailscale-ip>

🔐 Tailscale Remote Access

Tailscale adds:

Encrypted WireGuard connectivity

No need for port forwarding

Use MagicDNS:

ssh ubuntu@servername.tailnet-name.ts.net

Optional Enhancements

Enable server as a Tailscale Exit Node

Restrict SSH to Tailscale only

Use ACLs to limit access by device or user

Add subnet router mode to expose LAN services remotely

I can add any of these to the README on request.

⚙ Service Configuration
Nextcloud

Accessible via port 8080

Use external storage if desired (HDD/SSD)

Optional: add Redis cache for performance

Immich

Ideal for photo backup from mobile devices

Recommend mounting a dedicated photos volume

AdGuard Home

Set router DNS → server IP

Add blocklists (StevenBlack, OISD)

📦 Dependencies

Ubuntu Server LTS

Docker

Docker Compose

CasaOS

Tailscale

Containers:

Nextcloud

Immich

AdGuard Home

📘 Examples
Restart full stack:
docker compose down && docker compose up -d

Backup Nextcloud data:
sudo tar -czvf nextcloud_backup.tar.gz /var/lib/docker/volumes/nc_data

Connect via Tailscale:
tailscale status

🧰 Troubleshooting
Issue	Fix
Can't SSH remotely	Ensure Tailscale is running, check firewall
Slow performance	Use SSD for Nextcloud / Immich data
AdGuard not blocking	Router DNS not updated
Immich upload errors	Check database container or memory usage
👥 Contributors

You – Server builder & owner

ReadMeGen – README generator

📄 License

Choose any license. MIT or GPL are common choices.
