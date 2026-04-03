# 🧠 Jarvis Home AI Server — System Stabilization & DNS Deployment

**Date:** March 30, 2026  
**Session Type:** Infrastructure Stabilization + Networking + DNS  
**Host:** aiserver  
**OS:** Ubuntu Server 24.04 LTS  

---

# 🎯 Objective

Stabilize core system infrastructure and implement:

- Static IP configuration
- Reverse proxy routing (Traefik)
- Network-wide DNS resolution (AdGuard Home)
- Clean domain-based access to all services

---

# ⚙️ System Overview

## 🖥️ Host Configuration

- Hostname: `aiserver`
- Static IP: `192.168.12.220`
- Network Interface: `enp3s0`
- Remote Access: Tailscale (active)

---

## 💾 Storage

| Device        | Type  | Mount Point | Status   |
|--------------|------|------------|----------|
| 2 × 6TB HDD  | RAID1 | /data      | Healthy  |
| 2 × 1TB HDD  | RAID1 | /backup    | Healthy  |
| 223GB SSD    | OS    | /          | Healthy  |

---

## 🐳 Docker Architecture

### Core Services

- **Traefik** → Reverse Proxy (port 80)
- **Watchtower** → Automatic container updates

### Application Stack

- **Homepage** → Dashboard UI
- **Open WebUI** → AI Interface (Ollama)
- **Immich** → Photo Management
- **Portainer** → Container Management
- **Qdrant** → Vector Database

---

# 🌐 Networking

## 🔒 Static IP Configuration

Replaced DHCP with static configuration using Netplan.

### Key Fixes:
- Removed `50-cloud-init.yaml` (DHCP conflict)
- Disabled `systemd-resolved` override
- Configured static IP via `/etc/netplan/00-installer-config.yaml`

### Result: 192.168.12.220 (primary)

---

## 🌍 Reverse Proxy (Traefik)

Traefik configured to route services using domain-based rules.

### Active Routes:

| Domain                     | Service        |
|--------------------------|----------------|
| home.aiserver.local      | Homepage       |
| ai.aiserver.local        | Open WebUI     |
| immich.aiserver.local    | Immich         |
| admin.aiserver.local     | Portainer      |

---

## 🧠 DNS Server (AdGuard Home)

Deployed AdGuard Home to enable network-wide domain resolution.

### Deployment Details:

- Containerized via Docker
- DNS Port: `53`
- Admin UI: `3000`
- Data path: `/docker/adguard`

### Upstream DNS:
- `1.1.1.1`
- `8.8.8.8`

---

## 🔁 DNS Rewrites

Configured internal domain mapping:
home.aiserver.local → 192.168.12.220
ai.aiserver.local → 192.168.12.220
immich.aiserver.local → 192.168.12.220
admin.aiserver.local → 192.168.12.220

---

# 🧪 Troubleshooting & Fixes

## ❌ Issue: AdGuard failed to start
**Cause:** Port 53 in use by `systemd-resolved`  
**Fix:**
- Disabled service:
  ```
  sudo systemctl disable systemd-resolved
  sudo systemctl stop systemd-resolved

  Replaced /etc/resolv.conf with static DNS
  ```
## ❌ Issue: DNS not resolving on laptop

**Cause:** IPv6 DNS override from router
**Fix:**

Disabled IPv6 on Wi-Fi adapter
Forced DNS to 192.168.12.220
## ❌ Issue: Traefik routes not working

**Cause:** DNS not resolving domains
**Fix:** Implemented AdGuard DNS rewrites

## ❌ Issue: Portainer not accessible via Traefik

**Cause:** Container not on proxy network
**Fix:** Recreated container with Traefik labels and correct network

# ✅ Final Result
- All services accessible via clean domain names
- No need for port-based access
- DNS fully centralized and controlled
- Reverse proxy routing stable
- System behaves like a unified platform
## 🚀 Current Access
**Service URL**
- Dashboard	http://home.aiserver.local

- AI	http://ai.aiserver.local

- Photos	http://immich.aiserver.local

- Admin	http://admin.aiserver.local
## 🔮 Next Steps
- **Phase 2** — Remote Access
- Integrate Tailscale DNS
- Enable secure external access
- **Phase 3** — UX & Control Layer
- Enhance Homepage dashboard
- Add monitoring and service health
- **Phase 4** — Productization
- Backup automation
- App installer system 🔥
- Multi-user support
## 🧠 Summary

This session transformed the system from:

Local container environment

into:

A structured, domain-driven private platform

Key capabilities achieved:

Centralized DNS control
Clean service routing
Modular infrastructure foundation

## Status: ✅ Stable
Ready for next phase: YES

