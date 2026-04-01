# 💾 Jarvis Home AI Server — Storage Structure (v1.0)

---

## 🎯 Purpose

This document describes the full storage architecture of the system, including:

- Physical disks
- RAID configuration
- Mount points
- Data organization
- Docker storage behavior

## 🧠 Storage Overview

The system uses a hybrid storage model:

- SSD → Operating System
- HDD RAID1 → Primary Data
- HDD RAID1 → Backup Data

## 🖥️ Physical Disks

| Device | Size | Purpose |
|--------|------|--------|
| sde | 223GB | OS (Ubuntu) |
| sda + sdc | 2 × 6TB | Main Data (RAID1) |
| sdb + sdd | 2 × 1TB | Backup (RAID1) |


## 🧠 RAID Configuration

### Main Data Array

```
id="raid_main"
/dev/md0
```
- Type: RAID1 (Mirror)
- Size: ~5.5TB
- Mount: ```/data```

Backup Array
```
/dev/md1
```
- Type: RAID1 (Mirror)
- Size: ~931GB
- Mount: /backup
## ✅ RAID Status

Check with:
```
cat /proc/mdstat
```
Expected:
```
[UU]
```
Meaning:

- U = disk is active
- Both disks are healthy
## 📁 Mount Points
|Path|Purpose|
|----|-------|
|```/```|	OS (SSD)|
|```/data```|	Main storage|
|```/backup```|	Backup storage|
|```/docker```|	Application configs|
## 🔁 Persistent Mount Configuration

Defined in:
```
/etc/fstab
```
Example:
```
UUID=xxxx /data ext4 defaults 0 2
UUID=xxxx /backup ext4 defaults 0 2
```
## 🐳 Docker Storage Model

The system uses two storage types:

### 📂 Bind Mounts

Human-readable paths:
```
/docker/
/data/
```
Used for:

- Config files
- Media storage
- Persistent app data
### 📦 Named Volumes

Docker-managed storage:
```
/var/lib/docker/volumes/
```
Examples:

- open-webui_open-webui
- portainer_data
- qdrant_qdrant_storage
- immich_model-cache
## ⚠️ Important Distinction
|Type|Location|	Managed By|
|----|--------|-----------|
|Bind Mount|	```/docker```, ```/data```|	You|
|Named Volume|	```/var/lib/docker```|	Docker|
---
## 📸 Immich Storage
|Component|	Location|
|---------|---------|
|Photos|	|```/data/photos/library```|
|Database	|```/docker/immich/postgres```|
|Models	|```/docker/immich/model-cache```|

## 🧠 Storage Strategy
### Primary Data (```/data```)
- Photos (Immich)
- Future media
- AI datasets
### Backup (/backup)
- Snapshots
- System backups (future)
- Critical exports
## 🔐 Data Protection
- RAID1 provides redundancy
- If one disk fails → system continues running
- Replace disk → rebuild array
## 🔧 Useful Commands
Check disks
```
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
```
Check RAID
```
cat /proc/mdstat
```
Check mounts
```
df -h
```
Check volumes
```
docker volume ls
```
## ⚠️ Common Issues
### ❌ RAID degraded

Check:
```
cat /proc/mdstat
```
If not ```[UU]``` → disk failure

### ❌ Mount missing

Check:
```
df -h
```
Fix:
```
sudo mount -a
```
### ❌ Permission issues

Example:
```
sudo chown -R $USER:$USER /docker
```
## 🧠 Mental Model
```Disk → RAID → Filesystem → Mount → Docker → Application```

# 🚀 Summary
The storage system is:

- Redundant (RAID1)
- Organized (clear mount points)
- Scalable (add more arrays)
- Compatible with Docker
  
## 🔮 Future Improvements
- Automated backups to /backup
- Snapshot system
- Offsite backup (cloud or NAS)
- Monitoring (SMART, alerts)
