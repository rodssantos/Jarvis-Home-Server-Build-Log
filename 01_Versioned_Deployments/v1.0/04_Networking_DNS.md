# 🌐 Jarvis Home AI Server — Networking & DNS (v1.0)

# 🎯 Purpose

This document explains how networking, DNS, and routing work together in the system.

---

# 🧠 Network Overview

The system uses a **domain-based internal network architecture**.

Instead of accessing services via ports:


192.168.12.220:3001


You use clean domain names:


http://home.aiserver.local


---

# 🔗 Full Request Flow


User Device
↓
AdGuard DNS (192.168.12.220)
↓
Traefik (port 80)
↓
Docker Container (internal port)


---

# 🖥️ Server Network Configuration

## Static IP

```
IP: 192.168.12.220
Gateway: 192.168.12.1
```
Configured via Netplan:
```
dhcp4: no
addresses:
  - 192.168.12.220/24
```
# 🧠 DNS System (AdGuard Home)
Role

AdGuard acts as:

- Local DNS server
- Domain resolver
- Ad blocker (optional)
  
## DNS Server Address
```
192.168.12.220
```
All client devices must use this DNS.

## Domain Rewrites

Configured inside AdGuard:
```
home.aiserver.local   → 192.168.12.220
ai.aiserver.local     → 192.168.12.220
immich.aiserver.local → 192.168.12.220
admin.aiserver.local  → 192.168.12.220
```
## Upstream DNS

AdGuard forwards external queries to:

- Quad9 (DoH)
- Bootstrap DNS:
-- 9.9.9.10
-- 149.112.112.10
  
# ⚠️ Important Behavior
## Server DNS vs Client DNS

The server itself uses:
```
8.8.8.8
1.1.1.1
```
👉 Meaning:

- Server cannot resolve .local domains
- Only client devices can (if using AdGuard)

This is intentional and acceptable.

# 🌍 Reverse Proxy (Traefik)

Traefik listens on:
```
port 80
```
It routes requests based on domain name.

Example:
```
home.aiserver.local → Homepage container
```
# 🔗 Docker Network Design
## Key Network
```
proxy
```
All routed services must be connected to this network.

33 Rule

A container MUST:

1. Be on ```proxy``` network

2. Have Traefik labels

Otherwise:

❌ It will not be accessible via domain

# 🔥 Routing Example

Request:
```
http://ai.aiserver.local
```
Flow:

1.DNS resolves → 192.168.12.220

2.Request hits Traefik

3.Traefik finds matching rule:
```
Host(`ai.aiserver.local`)
```
4.Forwards to Open WebUI container (port 8080)

# ⚠️ Common Issues
## ❌ Domain not resolving

Check:

- Device DNS is set to 192.168.12.220
- AdGuard is running
- Rewrite rule exists
## ❌ Domain resolves but page doesn't load

Check:

- Container is running
- Container is on proxy network
- Traefik labels are correct
## ❌ Works on PC but not phone

Cause:

- Phone using router DNS instead of AdGuard

Fix:

- Set DNS manually or via router
## ❌ Server cannot resolve domains

Expected behavior.

To test DNS manually:
```
nslookup home.aiserver.local 192.168.12.220
```
# 🧠 Mental Model

If something breaks, always check in this order:

1.DNS (AdGuard)

2.Network (proxy)

3.Traefik (routing)

4.Container (service)

# 🚀 Summary

This system uses:

- Local DNS (AdGuard)
- Reverse proxy (Traefik)
- Docker networking (proxy network)
  
To create a clean, scalable, domain-based infrastructure.

