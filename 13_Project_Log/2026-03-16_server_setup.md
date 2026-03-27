# Jarvis Home Server Build Log - Section 2

**Date:** 2026-03-16  
**Host:** aiserver  
**OS:** Ubuntu Server  
**Primary User:** rodrigo
Time spent = 6 hours

--
# Step 11 — Reverse Proxy Infrastructure (Traefik)
## Objective

Create a centralized routing layer so all services can be accessed using human-readable hostnames instead of ports.

Example future services:


immich.aiserver.local
jellyfin.aiserver.local
cloud.aiserver.local
jarvis.aiserver.local
Traefik deployment

Directory created:

/docker/traefik

Configuration files:

traefik.yml
docker-compose.yml

Container launched with:

docker compose up -d
Verification

Container status:

docker ps

Expected container:

traefik:v3.0

## Traefik dashboard confirmed working at:

http://192.168.12.220:8080



# Step 12 — Internal DNS Resolution

## To allow local devices to resolve service hostnames, the Windows hosts file was modified.

File:

C:\Windows\System32\drivers\etc\hosts

Entry added:

192.168.12.220 immich.aiserver.local

DNS cache cleared:

ipconfig /flushdns

Result:

immich.aiserver.local → resolves to server
Step 13 — Immich AI Photo Server Deployment

Immich was deployed using Docker.

Directory:

/docker/immich

Containers launched:

immich_server
immich_machine_learning
immich_postgres
immich_redis

Startup command:

docker compose up -d

Container status verified with:

docker ps
Step 14 — Traefik Routing Configuration

Traefik configured to route requests based on hostname.

Routing rule:

Host(`immich.aiserver.local`)

Traffic flow:

Browser
   ↓
Traefik reverse proxy
   ↓
Immich container
Server routing test

Executed from server:

curl -I -H "Host: immich.aiserver.local" http://localhost

Expected result:

HTTP/1.1 200 OK

This confirmed:

reverse proxy working

Docker network connectivity correct

hostname routing functional

# Step 15 — Docker Network Validation

## Traefik and Immich were connected to a shared Docker network:

proxy

Inspection command:

docker network inspect proxy

Verified containers:

traefik
immich_server
# Step 16 — Immich Initial Configuration

Access URL:

http://immich.aiserver.local

Admin account created.

Configuration performed:

server settings verified

storage template engine enabled

machine learning features enabled

Enabled AI capabilities:

Smart Search
Facial Recognition
Object Detection

Machine learning currently runs on CPU (GPU acceleration planned later).

# Step 17 — Storage Mapping Verification

Docker volume mapping:

Host Path:        /data/photos/library
Container Path:   /usr/src/app/upload

Inspection command:

docker inspect immich_server

Confirmed bind mount:

Source: /data/photos/library
Destination: /usr/src/app/upload
Step 18 — Photo Upload Test

A test image was uploaded through the Immich web interface.

Storage verification command:

find /data/photos/library -type f | head

Result:

/data/photos/library/library/admin/2026/2026-03-16/...

This confirmed:

file persistence

RAID storage usage

correct Docker volume mapping

Current System State

Active infrastructure:

Ubuntu Server
│
├── Docker
├── Portainer
├── Traefik (reverse proxy)
│
└── Immich Stack
    ├── immich_server
    ├── immich_machine_learning
    ├── immich_postgres
    └── immich_redis

Service access:

http://immich.aiserver.local

Storage location:

/data/photos/library
What This Enables for Jarvis

The system now has the first component of the Visual Memory Layer described in the architecture.

Capabilities added:

photo timeline
face recognition
object search
visual memory storage

Example future queries:

"Show photos from the beach last summer"
"Find pictures with the dog"
"When did we visit the park?"
Next Infrastructure Targets

Upcoming components from the Jarvis roadmap:

AI Brain Layer
    Ollama
    Local models

Interface Layer
    Open WebUI

Knowledge Layer
    Document storage
    PDF chat system

Voice Layer
    Speech-to-text
    Text-to-speech

Automation Layer
    Home Assistant integration
