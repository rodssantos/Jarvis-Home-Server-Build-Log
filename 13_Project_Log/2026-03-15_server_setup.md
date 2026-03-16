# Jarvis Home Server Build Log

**Date:** 2026-03-15  
**Host:** aiserver  
**OS:** Ubuntu Server  
**Primary User:** rodrigo

---

# 1. System Installation

Installed Ubuntu Server on the dedicated AI / Home Server machine.

## Disk layout

| Disk | Purpose |
|-----|------|
| SSD (223GB) | Ubuntu OS |
| 2 × 6TB HDD | RAID1 Main Data Pool |
| 2 × 1TB HDD | RAID1 Backup Pool |

OS installation created:


/boot/efi
/
/home


Primary user:


rodrigo


---

# 2. RAID Arrays Detected

Existing RAID arrays were automatically detected.

| RAID Device | Type | Size | Purpose |
|-------------|------|------|--------|
| /dev/md127 | RAID1 | 5.5TB | Main data storage |
| /dev/md126 | RAID1 | 931GB | Backup storage |

Verified with:


cat /proc/mdstat
lsblk


---

# 3. Storage Mount Configuration

Mount points created:


/data
/backup


Mounted devices:


/dev/md127 → /data
/dev/md126 → /backup


Verified with:


df -h


Result:

| Mount | Size |
|------|------|
| /data | 5.5TB |
| /backup | 916GB |

---

# 4. Persistent Mounts

Configured persistent mounts in:


/etc/fstab


UUID retrieved using:


blkid


Example entries:


UUID=<md127-uuid> /data ext4 defaults 0 2
UUID=<md126-uuid> /backup ext4 defaults 0 2


Purpose: ensure storage mounts automatically on reboot.

---

# 5. Static Network Configuration

Changed DHCP configuration to static IP.

Network interface:


enp3s0


Server network identity:

| Parameter | Value |
|----------|------|
| IP | 192.168.12.220 |
| Subnet | /24 |
| Gateway | 192.168.12.1 |
| DNS | 8.8.8.8 |

Configuration file:


/etc/netplan/00-installer-config.yaml


Applied with:


sudo netplan apply


---

# 6. Hostname Configuration

Hostname standardized to:


aiserver


Command used:


sudo hostnamectl set-hostname aiserver


Hosts file updated:


/etc/hosts


Entry:


127.0.1.1 aiserver


---

# 7. Local Network Discovery

Installed Avahi for `.local` hostname resolution.


sudo apt install avahi-daemon


Enabled service:


sudo systemctl enable avahi-daemon
sudo systemctl start avahi-daemon


Server reachable via:


http://aiserver.local

ssh rodrigo@aiserver.local


---

# 8. Docker Environment

Docker runtime verified operational.

Detected resources:

| Resource | Value |
|--------|------|
| CPU | 8 cores |
| RAM | ~33GB |

Docker socket:


/var/run/docker.sock


---

# 9. Portainer Installation

Portainer CE installed for Docker container management.

Created volume:


docker volume create portainer_data


Deployed container:


docker run -d
-p 9000:9000
--name portainer
--restart=always
-v /var/run/docker.sock:/var/run/docker.sock
-v portainer_data:/data
portainer/portainer-ce:latest


Access URL:


http://192.168.12.220:9000


Environment connected:


Docker → Local


---

# 10. Current System Status

| Component | Status |
|---------|------|
| Ubuntu Server | ✅ Running |
| Static IP | ✅ Configured |
| Hostname | ✅ aiserver |
| RAID Storage | ✅ Mounted |
| Docker | ✅ Running |
| Portainer | ✅ Installed |
| Avahi (.local) | ✅ Enabled |

Hardware resources:


CPU: 8 cores
RAM: ~33GB
Storage: ~7.4TB usable


---

# 11. Next Steps

Planned next actions:


Create server directory structure:
/docker
/data
/backup


Then deploy services:

- Immich (photo cloud)
- Nextcloud (file storage)
- Paperless (document archive)
- Jellyfin (media server)
- Ollama + OpenWebUI (AI assistant)

---

✅ **End of Log Entry**
