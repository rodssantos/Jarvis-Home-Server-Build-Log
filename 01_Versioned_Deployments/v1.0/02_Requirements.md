
# ⚙️ FYNO — Your Private Home AI — Requirements (v1.0)

## 🖥️ Hardware Requirements

### Minimum (Functional System)

- CPU: 4 cores (Intel i5 / Ryzen 5 or equivalent)
- RAM: 16GB
- Storage:
  - 1 SSD (OS)
  - 1 HDD (data)
- Network: Gigabit Ethernet

---

### Recommended (Current System)

- CPU: Intel i7-12700K (or equivalent)
- RAM: 64GB
- GPU: NVIDIA RTX 3060 (12GB VRAM)
- Storage:
  - 223GB SSD → OS
  - 2 × 6TB HDD → RAID1 → `/data`
  - 2 × 1TB HDD → RAID1 → `/backup`

---

## 💾 Storage Layout

| Mount | Purpose |
|------|--------|
| `/` | OS (SSD) |
| `/data` | Main data (RAID1) |
| `/backup` | Backup storage (RAID1) |
| `/docker` | Application configs |

---

## 🧠 RAID Configuration

- RAID Type: RAID1 (mirror)
- Arrays:

| Device | Mount | Size |
|-------|------|------|
| md0 | /data | ~5.5TB |
| md1 | /backup | ~931GB |

Status should be:
[UU]

---

## 🐧 Operating System

- Ubuntu Server 24.04 LTS
- Minimal install (no GUI required)

---

## 🌐 Network Requirements

- Static IP required
- Router access (to set DNS)
- Local network access for all devices

---

## 🔌 Required Ports

| Port | Purpose |
|-----|--------|
| 80 | Traefik (HTTP routing) |
| 53 | DNS (AdGuard) |
| 3000 | AdGuard UI |
| 8080 | Traefik dashboard |
| 9000 | Portainer (optional direct access) |

---

## 🐳 Software Dependencies

Must be installed:

- Docker
- Docker Compose (v2+)

---

## 🧠 DNS Requirement

Network must allow:

- Setting custom DNS server
- Devices pointing to:
  192.168.12.220
  
---

## ⚠️ Important Notes

- ISP routers (like T-Mobile Home Internet) may limit port forwarding
- System is designed for **local network first**
- Remote access handled separately (future phase)

---

## 🚀 Optional (Recommended)

- Tailscale (for remote access)
- Portainer (for container UI)
- Watchtower (auto updates)

---

## 🧠 Philosophy

This system is designed to be:

- Private (local-first AI)
- Modular (Docker-based)
- Replicable (fully documented)
- Scalable (add services easily)
