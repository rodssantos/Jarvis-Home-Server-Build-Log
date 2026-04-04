# 🛠️ FYNO — Your Private Home AI — Installation Guide (v1.0)

## 🎯 Goal

Deploy a fully functional Jarvis AI server from a clean Ubuntu installation.

---

# 🧱 STEP 1 — Install Ubuntu Server

- Install **Ubuntu Server 24.04 LTS**
- Minimal installation
- Create user (e.g., `rodrigo`)

---

# 🔌 STEP 2 — Configure Static IP

Edit Netplan:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```
Example config:
```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: no
      addresses:
        - 192.168.12.220/24
      routes:
        - to: default
          via: 192.168.12.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```
Apply:
```
sudo netplan apply
```
# 💾 STEP 3 — Configure Storage (RAID + Mounts)
## Verify RAID
```
cat /proc/mdstat
```
Expected:
```
[UU]
```
## Mount points
```
/data
/backup
```
## Verify mounts
```lsblk ```

# 🧱 STEP 4 — Create Project Structure
```
sudo mkdir -p /docker
sudo chown -R $USER:$USER /docker
```
# 🐳 STEP 5 — Install Docker
```
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
```

## Add user to docker group:
```
sudo usermod -aG docker $USER
```
Logout and back in.

# 🌐 STEP 6 — Create Proxy Network
``` 
docker network create proxy
```
# 🚦 STEP 7 — Deploy Traefik

Create:
```
/docker/traefik/docker-compose.yml
```
Paste:
```
services:
  traefik:
    image: traefik:v3.0
    container_name: traefik
    restart: always
    command:
      - "--api.insecure=true"
      - "--api.dashboard=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    networks:
      - proxy

networks:
  proxy:
    external: true
```
Run:
```
cd /docker/traefik
docker compose up -d
```
# 🧠 STEP 8 — Deploy AdGuard (DNS)
```
mkdir -p /docker/adguard/work
mkdir -p /docker/adguard/conf
```
Create:
```
/docker/adguard/docker-compose.yml
```
```
services:
  adguard:
    image: adguard/adguardhome
    container_name: adguard
    restart: unless-stopped
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "3000:3000"
    volumes:
      - /docker/adguard/work:/opt/adguardhome/work
      - /docker/adguard/conf:/opt/adguardhome/conf
```
Run:
```
cd /docker/adguard
docker compose up -d
```
Access setup:
```
http://192.168.12.220:3000
```
# 🔁 STEP 9 — Configure DNS Rewrites

Inside AdGuard UI:

Add rewrites:
```
home.aiserver.local   → 192.168.12.220
ai.aiserver.local     → 192.168.12.220
immich.aiserver.local → 192.168.12.220
admin.aiserver.local  → 192.168.12.220
```
# 📊 STEP 10 — Deploy Homepage
```
mkdir -p /docker/homepage/config
```
Create:
```
/docker/homepage/docker-compose.yml
```
(Paste your homepage compose file)

Run:
```
cd /docker/homepage
docker compose up -d
```
# 🤖 STEP 11 — Deploy Open WebUI

Create:
```
/docker/open-webui/docker-compose.yml

(Paste your Open WebUI compose)
```
Run:
```
cd /docker/open-webui
docker compose up -d
```

# 📸 STEP 12 — Deploy Immich

Create .env file:
```
/docker/immich/.env
```
(Paste your env variables)

Then deploy:
```
cd /docker/immich
docker compose up -d
```
# 🧠 STEP 13 — Deploy Qdrant
```
cd /docker/qdrant
docker compose up -d
```
# 🌐 STEP 14 — Configure Devices DNS

Set ALL devices DNS to:
```
192.168.12.220
```
# ✅ STEP 15 — Verify System

Check:
```
docker ps
```
Access:
```
http://home.aiserver.local
http://ai.aiserver.local
http://immich.aiserver.local
```
# 🚀 DONE

## You now have a fully functional Jarvis AI Server.
# 🧠 Notes
- Traefik handles routing
- AdGuard handles DNS
- Docker handles services
- RAID handles storage
