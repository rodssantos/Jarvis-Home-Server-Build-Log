# 🧠 FYNO — Your Private Home AI — System Overview (v1.0)

## 🎯 Purpose

This system is a self-hosted, private AI + home services platform designed to:

- Run local AI (Open WebUI + Ollama)
- Manage personal media (Immich)
- Provide centralized dashboard access (Homepage)
- Enable internal domain routing (Traefik)
- Control DNS resolution (AdGuard Home)

All services are accessible via clean domain names inside the local network.

---

## 🧱 Core Architecture

The system follows a layered architecture:

### 1. Network Layer
- Static IP via Netplan
- Internal DNS via AdGuard Home
- Domain-based routing

### 2. Routing Layer
- Traefik reverse proxy
- Docker provider (label-based routing)

### 3. Application Layer
- Open WebUI (AI interface)
- Immich (photo system)
- Homepage (dashboard)
- Portainer (management)
- Qdrant (vector database)

### 4. Data Layer
- RAID1 storage arrays
- Docker volumes
- Bind-mounted persistent storage

---

## 🌐 Access Model

All services are accessed via local domains:

| Service | URL |
|--------|-----|
| Dashboard | http://home.aiserver.local |
| AI | http://ai.aiserver.local |
| Photos | http://immich.aiserver.local |
| Admin | http://admin.aiserver.local |

---

## 🔁 Request Flow

Device → AdGuard DNS → Traefik → Docker Container

---

## 🧠 Key Design Principles

- Local-first (no cloud dependency)
- Modular (each service isolated)
- Reproducible (fully documented deployment)
- Scalable (new services plug into proxy network)
- Clean routing (no ports exposed to users)

---

## 🚀 Current Status

- Core infrastructure: ✅ Stable
- DNS system: ✅ Operational
- Reverse proxy: ✅ Functional
- Services: ✅ Running
- Storage: ✅ Healthy (RAID1)

---

## 🔮 Future Evolution

- Remote access (Tailscale integration)
- Voice assistant layer
- Home automation integration
- Multi-user system
- Backup automation
