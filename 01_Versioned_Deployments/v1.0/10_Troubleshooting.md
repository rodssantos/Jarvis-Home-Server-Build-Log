# 🛠️ Jarvis Home AI Server — Troubleshooting (v1.0)
## - Also refer to general Troubleshooting at (
---

## 🎯 Purpose

This document provides common issues, root causes, and fixes for the Jarvis Home AI Server.

Use this as your **first stop** when something is not working.

---

## 🧠 Troubleshooting Method

Always debug in this order:

1. DNS (AdGuard)
2. Network (proxy)
3. Traefik (routing)
4. Container (service)
5. Logs

---

## 🌐 DNS Issues

### ❌ Domain not resolving

#### Symptoms

- `home.aiserver.local` does not load
- `nslookup` fails

#### Check

```
nslookup home.aiserver.local 192.168.12.220
```
Fix
- Ensure device DNS = 192.168.12.220
- Verify AdGuard is running:
```
docker ps | grep adguard
```
- Confirm rewrite exists in AdGuard
## ❌ Works on PC but not phone
Cause
- Phone using router DNS
Fix
- Set DNS manually on device
- Or configure router DNS
## 🚦 Traefik Issues
### ❌ Domain resolves but page does not load
Cause
- Traefik cannot reach container
Check
```
docker ps
docker network inspect proxy
```
Fix
- Ensure container is on proxy network
- Verify labels exist
### ❌ 502 Bad Gateway
Cause
- Wrong internal port
Fix

Check label:
```
traefik.http.services.<name>.loadbalancer.server.port=XXXX
```
Ensure it matches container port.

### ❌ Container not showing in Traefik
Cause
- Missing label
Fix
```
traefik.enable=true
```
## 🐳 Docker Issues
### ❌ Container not running
Check
```
docker ps -a
```
Fix
```
docker logs <container_name>
```
### ❌ Container keeps restarting
Cause
- Config error
- Missing environment variables
Fix
```
docker logs <container_name>
```
### ❌ Permission denied
Fix
```
sudo chown -R $USER:$USER /docker
```
## 💾 Storage Issues
### ❌ RAID degraded
Check
```
cat /proc/mdstat
```
If not `[UU]` → disk failure

### ❌ Mount missing
Check
```
df -h
```
Fix
```
sudo mount -a
```
### ❌ Disk not detected
Check
```
lsblk
```
## 🌍 Network Issues
### ❌ No internet
Check
```
ping -c 3 google.com
```
### ❌ Wrong IP
Check
```
ip a
```
## 📊 AdGuard Issues
### ❌ AdGuard not starting
Cause
- Port 53 in use
Fix
```
sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved
```
### ❌ DNS not applied
Fix
- Restart device
- Reconnect Wi-Fi
- Flush DNS cache
## 🧠 General Debug Commands
Check all containers
```
docker ps
```
Check logs
```
docker logs <container_name>
```
Restart stack
```
docker compose down
docker compose up -d
```
## 🔁 Mental Model
DNS → Traefik → Docker → Service

If something breaks:

- DNS issue → domain fails
- Traefik issue → routing fails
- Docker issue → service fails
## 🚀 Quick Fix Checklist
 - [ ] Container running
 - [ ] Correct network (proxy)
 - [ ] Traefik labels present
 - [ ] DNS rewrite exists
 - [ ] Ports correct
 - [ ] Logs checked
## 🧠 Pro Tips
- Always check logs first
- Always verify DNS before anything else
- Never assume — test each layer
## 🔮 Future Improvements
- Central logging system
- Monitoring dashboard
- Alerts for failures
