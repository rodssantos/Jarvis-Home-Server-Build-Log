# 🐳 Jarvis Home AI Server — Docker Stack (v1.0)

## 🎯 Purpose

This document describes all containers, their roles, and how they interact within the system.

---

## 🧠 Architecture Overview

The system is composed of multiple Docker containers organized into functional layers:

- Infrastructure (Traefik, AdGuard)
- Interface (Homepage, Open WebUI)
- Applications (Immich, Qdrant)
- Management (Portainer, Watchtower)

---

## 🔗 Network Design

### Core Network

```bash
proxy
```
Used for:

- Traefik routing
- Inter-service communication
## 📦 Active Containers
## 🚦 Traefik — Reverse Proxy
|Property|Value|
|--------|-----|
|Container|	traefik|
|Image	|traefik:v3.0|
|Ports	|80, 8080|
|Network|proxy|

Role:

- Routes all HTTP traffic
- Reads Docker labels
- Central access point
---
## 🌐 AdGuard — DNS Server
|Property|	Value|
|--------|-------|
|Container	|adguard|
|Image	|adguard/adguardhome|
|Ports|	53, 3000|

Role:

- DNS resolution
- Domain rewrites
- Network-wide control
---
## 🏠 Homepage — Dashboard

|Property|	Value|
|--------|-------|
|Container|	homepage|
|Port	3000| (internal)|
|Route	|home.aiserver.local|

Role:

- Central UI dashboard
- Quick access to services
---
## 🤖 Open WebUI — AI Interface
|Property|	Value|
|--------|-------|
|Container|	open-webui|
|Port|	8080|
|Route|	ai.aiserver.local|

Role:

- ChatGPT-like interface
- Connects to local AI models
---
## 📸 Immich — Photo System
|Property|	Value|
|--------|-------|
|Containers|	immich_server, immich_machine_learning|
|Port	|2283|
|Route|	immich.aiserver.local|

Role:

- Photo backup and management
- AI processing (faces, objects)
---
## 🧠 Immich Dependencies
|Service|	Purpose|
|-------|--------|
|PostgreSQL|	Database|
|Redis|	Cache|
|ML Container|	AI processing|
---
## 🧠 Qdrant — Vector Database
|Property|	Value|
|--------|-------|
|Container|	qdrant|
|Port	|6333|

Role:

- Stores embeddings
- Enables AI search capabilities
---
# 🛠️ Portainer — Container Manager
|Property|	Value|
|--------|-------|
|Container|	portainer|
|Port|	9000|
|Route|	admin.aiserver.local|

Role:

- Manage Docker visually
- Start/stop containers
- Inspect volumes
---
## 🔄 Watchtower — Auto Updates
|Property|	Value|
|--------|-------|
|Container|	watchtower|

Role:

- Automatically updates containers
- Keeps system current
---
## 📁 Container Grouping
Infrastructure
- Traefik
- AdGuard

Core Services
- Homepage
- Open WebUI
- Immich
- Qdrant

Management
- Portainer
- Watchtower
---
## 🔁 Data Flow Example
```User → DNS (AdGuard) → Traefik → Service Container```
## 🧠 Key Design Decisions
- Each service runs in its own container
- Shared proxy network for routing
- DNS-based service access
- Persistent storage via volumes or bind mounts
## ⚠️ Important Notes
- Some services expose ports but are accessed via domain
- Traefik only routes labeled containers
- Internal container ports are used for routing (not host ports)
## 🚀 Summary

The Docker stack is:

- Modular
- Scalable
- Easy to extend
- Fully containerized

## 🧠 How to Add New Services

To integrate a new service into the system:

1. Create Docker container
2. Attach it to proxy network
3. Add Traefik labels
4. Create DNS rewrite in AdGuard
