# Infrastructure Setup Guide

This guide explains how the **Jarvis Home Server infrastructure** was deployed.

The system is based on:

- Ubuntu Server
- Docker
- Traefik reverse proxy
- Immich photo AI server

---

# Step 1 — Install Docker

Install Docker engine.


sudo apt update
sudo apt install docker.io


Enable Docker:


sudo systemctl enable docker
sudo systemctl start docker


---

# Step 2 — Install Docker Compose


sudo apt install docker-compose-plugin


Verify:


docker compose version


---

# Step 3 — Install Portainer

Portainer provides container management UI.


docker volume create portainer_data


Run container:


docker run -d
-p 9000:9000
--name portainer
--restart=always
-v /var/run/docker.sock:/var/run/docker.sock
-v portainer_data:/data
portainer/portainer-ce:latest


Access:


http://SERVER_IP:9000


---

# Step 4 — Install Traefik Reverse Proxy

Create directory:


/docker/traefik


Create configuration files:


traefik.yml
docker-compose.yml


Start service:


docker compose up -d


Verify container:


docker ps


Traefik dashboard:


http://SERVER_IP:8080


---

# Step 5 — Configure Internal DNS

Edit hosts file on client machines.

Windows location:


C:\Windows\System32\drivers\etc\hosts


Add entry:


192.168.12.220 immich.aiserver.local


Flush DNS:


ipconfig /flushdns


---

# Step 6 — Deploy Immich

Create directory:


/docker/immich


Start containers:


docker compose up -d


Containers launched:


immich_server
immich_machine_learning
immich_postgres
immich_redis


---

# Step 7 — Verify Reverse Proxy

Test from server:


curl -I -H "Host: immich.aiserver.local" http://localhost


Expected:


HTTP/1.1 200 OK


---

# Step 8 — Create Admin Account

Open:


http://immich.aiserver.local


Create:

- admin user
- password
- email

---

# Step 9 — Enable AI Features

Inside Immich settings enable:


Smart Search
Facial Recognition
Object Detection


---

# Step 10 — Verify Storage Mapping

Docker bind mount:


Host: /data/photos/library
Container: /usr/src/app/upload


Verify with:


docker inspect immich_server


---

# Step 11 — Test Upload

Upload a test image.

Verify storage:


find /data/photos/library -type f


Example result:


/data/photos/library/library/admin/2026/2026-03-16/photo.png


---

# Infrastructure Status

Running services:


Traefik
Portainer
Immich
Postgres
Redis
Machine Learning


Access URL:


http://immich.aiserver.local


---

# Next Steps

Planned services:

- Ollama (local LLMs)
- Open WebUI
- Paperless-ngx
- Nextcloud
- Jellyfin
