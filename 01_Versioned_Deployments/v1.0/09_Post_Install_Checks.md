# ✅ Jarvis Home AI Server — Post Install Checks (v1.0)

---

## 🎯 Purpose

This document provides a checklist to verify that all system components are functioning correctly after deployment.

---

## 🧠 Verification Layers

The system should be validated in this order:

1. System (OS + Network)
2. Storage (RAID + Mounts)
3. Docker (Containers + Networks)
4. DNS (AdGuard)
5. Routing (Traefik)
6. Applications

---

## 🖥️ System Checks

### Check IP Address

```
ip a
```
Expected:
```
192.168.12.220
```
Check Internet Connectivity
```
ping -c 3 google.com
```
## 💾 Storage Checks
### Check disks and mounts
```
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
```
### Check RAID status
```
cat /proc/mdstat
```
Expected:
```
[UU]
```
### Check mount usage
```
df -h
```
Ensure:

- `/data` is mounted
- `/backup` is mounted
## 🐳 Docker Checks
### Check running containers
```
docker ps
```
Expected containers:

- traefik
- adguard
- homepage
- open-webui
- immich_server
- immich_machine_learning
- qdrant
- portainer
- watchtower

### Check Docker networks
```
docker network ls
```
Ensure:

'proxy'

exists.

## 🌐 DNS Checks (AdGuard)
### Test DNS resolution (from client device)
```
nslookup home.aiserver.local
```
Expected:
```
192.168.12.220
```
### Test from server (manual DNS)
```
nslookup home.aiserver.local 192.168.12.220
```
### 🚦 Traefik Checks
## Access dashboard
```
http://192.168.12.220:8080
```
Verify:

- Routers exist
- Services are healthy
### Check logs
```
docker logs traefik
```
## 🌍 Application Access

Open in browser:

- http://home.aiserver.local
- http://ai.aiserver.local
- http://immich.aiserver.local
- http://admin.aiserver.local
### Expected Behavior
|Service|	Expected|
|----|-----|
|Homepage|	Dashboard loads|
|Open WebUI|	Chat interface|
|Immich|	Login screen|
|Portainer	|Container UI|
##🔁 End-to-End Test

Flow:

`Device → DNS → Traefik → Container`

If ALL services load → system is fully operational.

## ⚠️ Troubleshooting Quick Checks
### ❌ Domain not resolving
- Check DNS settings on device
- Ensure AdGuard is running
### ❌ Page not loading
Check container status:
```
docker ps
```
###❌ Traefik routing issue
- Check labels
- Check proxy network
### ❌ Service crashes
Check logs:
```
docker logs <container_name>
```
## 🧠 Health Checklist
|Component	|Status|
|------|------|
|Network	|✅|
|RAID	|✅|
|Docker	|✅|
|DNS	|✅|
|Traefik	|✅|
|Services	|✅|
## 🚀 Final Validation

If all checks pass:

✅ System is fully deployed
✅ All services reachable
✅ Infrastructure stable

## 🔮 Next Steps
- Enable backups
- Configure monitoring
- Add new services
- Setup remote access
