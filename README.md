# ☁️ MySpace - *Self-Hosted Cloud Storage Server*

### It is a Personal Cloud Storage *(like Google Drive)* running on Raspberry Pi.

[![Ubuntu](https://img.shields.io/badge/OS-Ubuntu_Server_24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docker.com/)
[![Nextcloud](https://img.shields.io/badge/Nextcloud-Cloud_Storage-0082C9?style=for-the-badge&logo=nextcloud&logoColor=white)](https://nextcloud.com/)
[![Nginx](https://img.shields.io/badge/Nginx-Reverse_Proxy-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org/)
[![Grafana](https://img.shields.io/badge/Grafana-Monitoring-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)

---

## 📖 Description

**MySpace** is a fully self-hosted cloud storage server built and deployed on a **Raspberry Pi 4B**. It replicates the core functionality of services like Google Drive and Dropbox - but runs entirely on your own hardware, under your full control.

Users can upload and download files, sync data, share files via links, and access the system from any browser or mobile device - all through a clean, port-free URL exposed to the public internet.

This project is built with production-grade practices: Docker containerization, Nginx reverse proxying, public tunneling via Ngrok, real-time monitoring using Prometheus and Grafana, automated daily backups, and a full disaster recovery system.

---

## 🎥 Demo

You can watch the demo here: [View Demo](https://drive.google.com/drive/folders/1CNaFbuQKqthFmF4LDtYGI-Dl8gyYOGGk?usp=drive_link)

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| **Operating System** | Ubuntu Server 24.04 LTS (Raspberry Pi) |
| **Containerization** | Docker + Docker Compose |
| **Cloud Platform** | Nextcloud |
| **Web Server / Reverse Proxy** | Nginx |
| **Database** | PostgreSQL 15 |
| **Monitoring** | Prometheus + Grafana |
| **Public Access** | Ngrok |
| **Backup & Automation** | Bash Scripting + Cron |

---

## 🏗️ Architecture

```
                        ┌───────────────────────────┐
                        │         Internet          │
                        └────────────┬──────────────┘
                                     │
                        ┌────────────▼──────────────┐
                        │           Ngrok           │
                        │  (Public HTTPS Tunnel)    │
                        └────────────┬──────────────┘
                                     │
                        ┌────────────▼──────────────┐
                        │     Nginx Reverse Proxy   │
                        │     (Port 80 → 8080)      │
                        └────────────┬──────────────┘
                                     │
               ┌─────────────────────▼──────────────────────┐
               │            Docker Containers               │
               │                                            │
               │  ┌─────────────┐   ┌────────────────────┐  │
               │  │  Nextcloud  │   │    PostgreSQL DB   │  │
               │  │  (Port 8080)│◄──│    (nextcloud_db)  │  │
               │  └──────┬──────┘   └────────────────────┘  │
               │         │                                  │
               │  ┌──────▼──────┐   ┌────────────────────┐  │
               │  │  Prometheus │   │      Grafana       │  │
               │  │  (Port 9090)│──►│    (Port 3000)     │  │
               │  └─────────────┘   └────────────────────┘  │
               └─────────────────────┬──────────────────────┘
                                     │
                        ┌────────────▼──────────────┐
                        │ USB Storage (/mnt/storage)│
                        │  (Persistent Data Layer)  │
                        └───────────────────────────┘
```

---

## ✨ Features

| Category | Details |
|---|---|
| ☁️ **Cloud Storage** | Multi-user system, file upload/download, secure sharing via links |
| 🌐 **Reverse Proxy** | Clean URL with no port exposure, request routing via Nginx |
| 🌍 **Public Access** | Internet access via Ngrok - no port forwarding required |
| 📊 **Monitoring** | CPU, RAM, Disk, and container metrics via Grafana dashboards |
| 🔄 **Backup System** | Automated daily backups with 30-day retention policy |
| ♻️ **Disaster Recovery** | Full restore from backup archives with automated permission handling |


---

## 📸 Screenshots
- Login Page
<img width="1920" height="1080" alt="Login Page" src="https://github.com/user-attachments/assets/7ed689b2-6d31-4041-8d6c-89ffac03fd08" />

- Dashboard
<img width="1920" height="1080" alt="Dashboard" src="https://github.com/user-attachments/assets/3ee34f3a-63d1-48c3-828a-4acd34815a5b" />

- Prometheus
<img width="1920" height="1080" alt="Prometheus" src="https://github.com/user-attachments/assets/62f3333c-5a9a-4c1d-8f1c-55a7445db87b" />

- Grafana
<img width="1920" height="1080" alt="Grafana" src="https://github.com/user-attachments/assets/a6a40b45-67d4-426d-ac85-96ee5c4dba05" />

---

## 📋 Prerequisites

Before you begin, ensure you have the following:

- Raspberry Pi 4B
- MicroSD card (32GB or more)
- USB Pen Drive / SSD (for storage)
- Power adapter
- Laptop
- Mobile hotspot (WiFi)

---

## 🚀 Setup Guide

---

## Phase 1 - First Boot (Raspberry Pi Setup)

**Goal:** Boot Raspberry Pi → Connect via SSH → Get terminal access.

### Step 0 - Install OS on SD Card

Download **Raspberry Pi Imager** from: https://www.raspberrypi.com/software/

In Raspberry Pi Imager:

- Click **Choose OS**
- Select: `Other general-purpose OS → Ubuntu → Ubuntu Server 24.04 (64-bit)`
- Click **Choose Storage** → select your SD card

### Step 1 - Pre-configure (Press ⚙️ icon in Imager)

Set the following before flashing:

**Username & Password:**
```
username: dhruv
password: (your choice)
```

**WiFi Setup:**
```
SSID: your hotspot name
Password: your hotspot password
```

**Hostname:**
```
MySpace
```

**Enable SSH:** ✔ (Use password authentication)

Click **Save** → Click **Write**

### Step 2 - Boot Raspberry Pi

- Insert SD card into Pi
- Connect power
- Wait ~2–3 minutes (first boot takes time)

### Step 3 - Find Raspberry Pi IP Address

On your laptop terminal, run:
```bash
ipconfig
```
Note your laptop's IP address. Open **Angry IP Scanner**, set the IP range for your network, and scan. Find the Raspberry Pi's IP address.

### Step 4 - SSH into the Pi

Using PuTTY or your terminal:
```bash
ssh dhruv@<pi-ip>
# Example:
ssh dhruv@10.232.41.151
```

**✅ Success indicator:**
```
dhruv@hostname:~$
```

### Step 5 - Basic Setup

```bash
sudo apt update
sudo apt upgrade -y
```

**Set timezone:**
```bash
sudo timedatectl set-timezone Asia/Kolkata
```

> ⚠️ **Note:** RealVNC will not work with Ubuntu Server OS as it does not support VNC.

---

## Phase 2 - Prepare Server + Install Docker

**Goal:** Make your server ready to run real applications (Nextcloud, monitoring, etc.)

### Step 1 - Install Basic Utilities

```bash
sudo apt install -y curl wget git unzip htop net-tools
```

These tools help with: downloads (`curl`/`wget`), monitoring (`htop`), networking (`net-tools`).

### Step 2 - Configure Firewall

```bash
sudo apt install ufw -y

sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

**Verify firewall status:**
```bash
sudo ufw status
```

### Step 3 - Install Docker

```bash
curl -fsSL https://get.docker.com | sudo sh
```

**Add your user to the docker group:**
```bash
sudo usermod -aG docker $USER
```

> ⚠️ **IMPORTANT:** Log out and reconnect SSH for the group change to take effect.

```bash
exit
# Then reconnect:
ssh dhruv@<pi-ip>
```

**Test Docker:**
```bash
docker --version
docker run hello-world
```
> ✅ If `hello-world` runs → Docker is installed correctly 🎉

### Step 4 - Install Docker Compose

```bash
sudo apt install docker-compose-plugin -y
docker compose version
```

---

## Phase 3 - Setup USB Storage

**Goal:** Mount USB drive → Use it as permanent storage → Integrate with Nextcloud.

### Step 1 - Plug in USB

Insert the pen drive into the Raspberry Pi, then check:
```bash
lsblk
```

You'll see something like:
```
sda     8:0    1   58G
└─sda1  8:1    1   58G
```
> 👉 Your USB = `sda1`

### Step 2 - Format USB

> ⚠️ This will erase all existing data on the USB.

```bash
sudo mkfs.ext4 /dev/sda1
```

### Step 3 - Create Mount Point

```bash
sudo mkdir -p /mnt/storage
```

### Step 4 - Mount USB

```bash
sudo mount /dev/sda1 /mnt/storage
```

**Verify:**
```bash
df -h
```
> 👉 You should see `/mnt/storage` listed.

### Step 5 - Make Mount Permanent (VERY IMPORTANT)

**Get the UUID of your USB:**
```bash
sudo blkid
# Example output: /dev/sda1: UUID="abcd-1234"
```

**Edit `/etc/fstab`:**
```bash
sudo vi /etc/fstab
```

**Add this line:**
```
UUID=abcd-1234 /mnt/storage ext4 defaults 0 2
```

Save and exit.

### Step 6 - Set Permissions

```bash
sudo chown -R dhruv:dhruv /mnt/storage
```

> ✅ Storage is now mounted, persistent across reboots, and ready for Nextcloud.

---

## Phase 4 - Deploy Your Cloud Server (Nextcloud + PostgreSQL)

**Goal:** Deploy Nextcloud and PostgreSQL via Docker.

### Step 1 - Create Project Folder

```bash
mkdir ~/nextcloud-server
cd ~/nextcloud-server
```

### Step 2 - Create Docker Compose File

```bash
vi docker-compose.yml
```

Paste the following:

```yaml
version: '3.8'

services:
  db:
    image: postgres:15
    container_name: nextcloud_db
    restart: always
    environment:
      POSTGRES_DB: nextcloud
      POSTGRES_USER: ncuser
      POSTGRES_PASSWORD: ncpass
    volumes:
      - ./db:/var/lib/postgresql/data

  app:
    image: nextcloud
    container_name: nextcloud_app
    restart: always
    ports:
      - "8080:80"
    environment:
      POSTGRES_HOST: db
      POSTGRES_DB: nextcloud
      POSTGRES_USER: ncuser
      POSTGRES_PASSWORD: ncpass
    volumes:
      - /mnt/storage:/var/www/html
    depends_on:
      - db
```

### Step 3 - Start the Server

```bash
docker compose up -d
```

### Step 4 - Check Running Containers

```bash
docker ps
```

You should see: `nextcloud_app` and `nextcloud_db`

### Step 5 - Access Your Cloud

Open in browser:
```
http://<pi-ip>:8080
# Example: http://192.168.43.120:8080
```

### Step 6 - First Setup (Admin Account)

On the Nextcloud setup page:

**Create admin account:**
```
Username: dhruv
Password: (choose a strong password)
```

**Database configuration:**
```
Database: PostgreSQL
User:     ncuser
Password: ncpass
Database: nextcloud
Host:     db
```

Click **Install** to complete setup.

---

## Phase 5 - Reverse Proxy (Remove :8080 from URL)

**Goal:** Make your server accessible via a clean URL without exposing ports.

```
http://<pi-ip>:8080   ❌
          ↓
http://<pi-ip>        ✅
```

**How it works:**
```
User → Nginx (port 80) → Nextcloud (port 8080)
```

### Step 1 - Install Nginx

```bash
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```

**Test:** Open `http://<pi-ip>` — you should see the Nginx default page.

### Step 2 - Create Reverse Proxy Config

```bash
sudo vi /etc/nginx/sites-available/nextcloud
```

Paste:
```nginx
server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### Step 3 - Enable Config

```bash
sudo ln -s /etc/nginx/sites-available/nextcloud /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
```

### Step 4 - Restart Nginx

```bash
sudo systemctl restart nginx
```

### Step 5 - Add Your IP as Trusted Domain

```bash
sudo vi /mnt/storage/config/config.php
```

Add your IP to the trusted domains array:
```php
'trusted_domains' =>
array (
  0 => 'localhost',
  1 => '10.232.41.151',
),
```

> To save and exit vi: press `Esc`, then type `:wq` and press Enter.

### Step 6 - Test

Open `http://<pi-ip>` — Nextcloud should open without `:8080` 🎉

---

## Phase 6 - Public Access via Ngrok

**Goal:** Expose your local server to the internet.

```
http://your-pi  (local)
       ↓
https://random.ngrok.io  (public)
```

### Step 1 - Create Ngrok Account

Go to https://ngrok.com/ → Sign up → Login → Create an Auth Token → Copy it.

### Step 2 - Install Ngrok on Raspberry Pi

> Run from your home directory:

```bash
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-arm64.tgz
tar -xvzf ngrok-v3-stable-linux-arm64.tgz
sudo mv ngrok /usr/local/bin/
```

**Verify installation:**
```bash
ngrok version
```

### Step 3 - Add Auth Token

```bash
ngrok config add-authtoken YOUR_TOKEN
```

### Step 4 - Start Tunnel

```bash
# With Nginx (recommended):
ngrok http 80

# Without Nginx:
ngrok http 8080
```

**Output will look like:**
```
Forwarding  https://abc123.ngrok.io → http://localhost:80
```

### Step 5 - Add Ngrok Domain as Trusted

Every time Ngrok generates a new URL, you must add it to Nextcloud's trusted domains:

```bash
sudo vi /mnt/storage/config/config.php
```

```php
'trusted_domains' =>
array (
  0 => 'localhost',
  1 => '10.232.41.151',
  2 => 'abc123.ngrok.io',
),
```

> ⚠️ Do **not** include `https://` in the trusted domain value.

> To save and exit vi: press `Esc`, then type `:wq` and press Enter.

> ⚠️ **Free Ngrok Limitations:**
> - URL changes every restart
> - Limited bandwidth
> - Keep terminal open — closing it stops the tunnel

### Step 6 - Access from Anywhere

Open `https://abc123.ngrok.io` → Your Nextcloud is now live on the internet 🌍

---

## Phase 7 - Monitoring (Prometheus + Grafana)

**Goal:** To monitor CPU usage, RAM usage, Disk usage, Docker containers.

**Step 1 - Create monitoring compose file:**

```bash
cd ~/nextcloud-server
vi monitoring.yml
```

Paste:
```yaml
version: '3'

services:
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    restart: always

  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    restart: always
  
  node_exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"
    restart: always
```

**Step 2 - Run Node Exporter (collects CPU, RAM, Disk):**

```bash
docker run -d \
  --name=node_exporter \
  -p 9100:9100 \
  prom/node-exporter
```

**Step 3 - Create Prometheus config:**

```bash
vi ~/nextcloud-server/prometheus.yml
```

Paste (mind the indentation):
```yaml
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['node_exporter:9100']
```

**Step 4 - Start monitoring stack:**

```bash
docker compose -f monitoring.yml up -d
```

**Access:**
```
http://<pi-ip>:3000  →  Grafana
http://<pi-ip>:9090  →  Prometheus
```

> ✅ Default Grafana credentials: `admin` / `admin` (you'll be prompted to change the password on first login).

**Step 5 - Connect Prometheus to Grafana:**

- Go to ⚙️ Settings → Data Sources → Add data source → Select **Prometheus**
- Set URL: `http://prometheus:9090`
- Click **Save & Test** → Should show ✅ success

**Step 6 - Import Dashboard:**

- Click **+** → **Import**
- Enter Dashboard ID: `1860` *(Node Exporter Full)*
- Select Prometheus as the data source → **Import**

---

## Phase 8 - Automated Backup System

**Goal:** To create Automated daily backups with 30-day retention policy.

**Step 1 - Give storage ownership to your user:**

```bash
sudo chown -R dhruv:dhruv /mnt/storage
sudo apt install pv
```

**Step 2 - Create backup directory:**

```bash
mkdir -p ~/backups
```

**Step 3 - Create backup script:**

```bash
vi ~/backup.sh
```

Paste:
```bash
#!/bin/bash

BACKUP_DIR="/home/dhruv/backups"
SOURCE="/mnt/storage"

mkdir -p $BACKUP_DIR

echo "Backup started at $(date)"

tar -czf - $SOURCE | pv > $BACKUP_DIR/backup_$(date +%F).tar.gz

echo "Backup completed at $(date)"

# Delete backups older than 30 days
find $BACKUP_DIR -type f -name "backup_*.tar.gz" -mtime +30 -exec rm {} \;
```

**Make it executable and run manually:**

```bash
chmod +x ~/backup.sh
./backup.sh
```

**Step 4 - Automate with cron:**

```bash
crontab -e
```

Add this line:
```
0 2 * * * /home/dhruv/backup.sh
```

> 👉 This runs a backup daily at 2 AM.

---

## Phase 9 - Disaster Recovery (Restore from Backup)

**Goal:** To fully restore system from backup archives with automated permission handling.

**Step 1 - Create restore script:**

```bash
nano ~/restore.sh
```

Paste:
```bash
#!/bin/bash

BACKUP_FILE=$1
PROJECT_DIR="/home/dhruv/nextcloud-server"

if [ -z "$BACKUP_FILE" ]; then
  echo "Usage: ./restore.sh <backup-file>"
  exit 1
fi

echo "Stopping Nextcloud containers..."
cd $PROJECT_DIR
docker compose down

echo "Cleaning current data..."
sudo rm -rf /mnt/storage/*

echo "Restoring backup from $BACKUP_FILE ..."
sudo tar -xzvf $BACKUP_FILE -C /

echo "Fixing permissions..."
sudo chown -R dhruv:dhruv /mnt/storage

echo "Fixing Nextcloud container permissions..."
docker run --rm \
  -v /mnt/storage:/var/www/html \
  nextcloud chown -R www-data:www-data /var/www/html

echo "Starting containers..."
docker compose up -d

echo "Restore completed successfully!"
```

**Make it executable:**
```bash
chmod +x ~/restore.sh
```

**Step 2 - How to restore:**

```bash
# List available backups:
ls ~/backups

# Run restore:
./restore.sh /home/dhruv/backups/backup_2026-04-07.tar.gz
```

**What the restore script does:**
- Stops all containers
- Deletes current data
- Extracts the backup archive
- Fixes file permissions
- Restarts the system

> ⚠️ **Important Rules:**
> - Always provide the **full path** of the backup file
> - Current data will be **overwritten** by the backup
> - The script handles container shutdown automatically - do not stop containers manually before running it

**Testing Disaster Recovery (Simulation):**
```bash
cd ~/nextcloud-server
docker compose down
sudo rm -rf /mnt/storage/*

# Then test restore:
./restore.sh <backup-file>
```

> ⚠️ Do NOT move the mount point for testing - just clear the data as shown above.

---

## 📁 Project Folder Structure

```
/home/dhruv/
├── nextcloud-server/
│   ├── docker-compose.yml      # Nextcloud + PostgreSQL
│   ├── monitoring.yml          # Prometheus + Grafana
│   └── prometheus.yml          # Prometheus scrape config
│
├── backups/                    # Automated backup archives
│   └── backup_YYYY-MM-DD.tar.gz
│
├── backup.sh                   # Backup automation script
└── restore.sh                  # Disaster recovery script
```

---

## 🔗 Service URLs

| Service | URL |
|---|---|
| Nextcloud | `http://<pi-ip>` or `https://<ngrok-url>` |
| Grafana | `http://<pi-ip>:3000` |
| Prometheus | `http://<pi-ip>:9090` |

---

## ✅ Final Status

| Component | Status |
|---|---|
| Raspberry Pi Linux Server | ✅ |
| Nextcloud Cloud Storage | ✅ |
| Docker Deployment | ✅ |
| Nginx Reverse Proxy | ✅ |
| Ngrok Public Access | ✅ |
| Multi-User System | ✅ |
| Monitoring (Grafana + Prometheus) | ✅ |
| Backup Automation | ✅ |
| Disaster Recovery | ✅ |

---

## 🏁 Conclusion

**MySpace** demonstrates the full lifecycle of deploying a production-grade, self-hosted cloud infrastructure from the ground up - starting from bare hardware and ending with a monitored, internet-accessible, and fault-tolerant storage system.

This project covers real-world DevOps practices including:
- **Containerization** with Docker
- **Reverse proxying** with Nginx
- **Secure public tunneling** with Ngrok
- **Observability** with Prometheus and Grafana
- **Data resilience** through automated backups and disaster recovery

---


#### Built with ❤️ by Dhruv Patel
