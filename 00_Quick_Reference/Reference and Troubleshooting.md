# ⚡ Jarvis Server — Quick Reference

---

# 🌐 1. CORE ACCESS (USE THIS FIRST)

| Service | URL |
|--------|-----|
| Dashboard | http://home.aiserver.local |
| AI | http://ai.aiserver.local |
| Photos | http://immich.aiserver.local |
| Admin | http://admin.aiserver.local |
| DNS Admin | http://192.168.12.220:3000 |

---

# 🖥️ 2. SERVER INFO

- Hostname: aiserver
- IP Address: 192.168.12.220
- Interface: enp3s0

---

# 🔁 3. ROUTING FLOW (MENTAL MODEL)
Device → DNS (AdGuard) → Traefik → Container

If something breaks, check in this order:
1. DNS resolving?
2. Container running?
3. Traefik routing?

---

# 🌍 4. DNS SUMMARY (AdGuard)

## DNS Server
192.168.12.220

## Rewrites
- home.aiserver.local → 192.168.12.220
- ai.aiserver.local → 192.168.12.220
- immich.aiserver.local → 192.168.12.220
- admin.aiserver.local → 192.168.12.220

## Upstream DNS
- 1.1.1.1
- 8.8.8.8

---

# 🐳 5. DOCKER COMMANDS (MOST USED)

## Check running containers
docker ps
## Start everything
docker compose up -d
## Stop everything
docker compose down
## Restart a container
docker restart <container_name>

### View logs
docker logs -f <container_name>

---

# 🔧 6. SYSTEM COMMANDS
# Edit docker compose
nano docker-compose.yml
# Check ports in use
sudo lsof -i :80

sudo lsof -i :53
# Restart networking
sudo netplan apply

---

# ⚠️ 7. COMMON ISSUES & FIXES

## ❌ DNS not working
Make sure device DNS = 192.168.12.220

Disable IPv6 if overriding
## ❌ AdGuard not starting

Port 53 conflict:

sudo systemctl disable systemd-resolved

sudo systemctl stop systemd-resolved

## ❌ Domain not loading
Check AdGuard rewrite
Check DNS resolution:
nslookup home.aiserver.local

## ❌ Service not loading
Check container:
docker ps
Check logs:
docker logs <container_name>

## ❌ Traefik issue
Check labels
Check container network (proxy)

---

# 🧠 8. SERVICES OVERVIEW

| Service | Purpose |
|---------|---------|
| Traefik | Reverse proxy |
| AdGuard | DNS server|
|Homepage | Dashboard |
| Open WebUI | AI interface |
| Immich | Photos |
|Portainer|Docker UI|
|Qdrant|Vector DB|
|Watchtower|Auto updates|

---

# 🔌 9. IMPORTANT PORTS
|Port|Service|
|----|-------|
|53|DNS (AdGuard)|
|80|Traefik|
|8080 | Traefik dashboard|
|3000|AdGuard UI|
|3001 | Homepage direct port|
|9000 | Portainer direct port|
|2283 | Immich direct port|
|6333 | Qdrant direct port|

---

# 🚀 10. QUICK RECOVERY CHECKLIST

### If something is broken:

1.Can I ping the server?

2.Does DNS resolve?

3.Is container running?

4.Is Traefik routing?

5.Check logs

---

# 🧠 RULE OF THUMB

If it doesn’t load:

👉 It’s ALWAYS one of these:

DNS

Container

Traefik
