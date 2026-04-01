# 🛠️ Jarvis Home AI Server — Installation Guide (v1.0)

## 🎯 Goal

Deploy a fully functional Jarvis AI server from a clean Ubuntu installation.

---

# 🧱 STEP 1 — Install Ubuntu Server

- Install **Ubuntu Server 24.04 LTS**
- Minimal installation
- Create user (e.g., `rodrigo`)

---

# 🔌 STEP 2 — Configure Static IP

Edit Netplan:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```
Example config:
```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: no
      addresses:
        - 192.168.12.220/24
      routes:
        - to: default
          via: 192.168.12.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```
Apply:
```
sudo netplan apply
```
# 💾 STEP 3 — Configure Storage (RAID + Mounts)
## Verify RAID
```
cat /proc/mdstat
```
Expected:
```
[UU]
```
## Mount points
```
/data
/backup
```
## Verify mounts
```lsblk ```

# 🧱 STEP 4 — Create Project Structure
```
sudo mkdir -p /docker
sudo chown -R $USER:$USER /docker
```
# 🐳 STEP 5 — Install Docker
```
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
```

## Add user to docker group:
```
sudo usermod -aG docker $USER
```
Logout and back in.

# 🌐 STEP 6 — Create Proxy Network
``` 
docker network create proxy
```
