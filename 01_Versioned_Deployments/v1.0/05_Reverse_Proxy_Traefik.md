# 🚦 Jarvis Home AI Server — Reverse Proxy (Traefik) (v1.0)

## 🎯 Purpose

Traefik is the reverse proxy responsible for routing all incoming requests to the correct service based on domain names.

---

# 🧠 What Traefik Does

Traefik listens for HTTP requests and decides:


Which container should receive this request?


Instead of using ports, routing is based on domain names.

---

# 🌐 Entry Point

Traefik listens on:

``` id="z8q2pl"
port 80
```
Defined as:
```
entryPoints:
  web:
    address: ":80"
```
# 🔌 Dashboard

Traefik dashboard is enabled:
```
http://192.168.12.220:8080
```
⚠️ Note: This is insecure (no auth). Safe only inside local network.

# 🔗 Docker Integration

Traefik uses Docker provider:
```
providers:
  docker:
    exposedByDefault: false
```
# 🔥 Important Rule

Containers are NOT exposed unless explicitly enabled
## 🧩 How Routing Works

Traefik reads container labels to define routing rules.

# 🧠 Example: Homepage
```
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.home.rule=Host(`home.aiserver.local`)"
  - "traefik.http.routers.home.entrypoints=web"
  - "traefik.http.services.home.loadbalancer.server.port=3000"
  - "traefik.docker.network=proxy"
```
# 🔍 Breakdown
Enable routing
```
traefik.enable=true
```
Define domain
```
Host(`home.aiserver.local`)
```
Entry point
```
web (port 80)
```
Internal container port
```
3000
```
👉 This is NOT the host port — it's the container port

Network
```proxy```

👉 Required for Traefik to reach the container

# 🔗 Required Conditions for Routing

A container MUST:

1. Have ```traefik.enable=true```
2. Be connected to ```proxy``` network
3. Define a router rule (Host)
4. Define service port
   
# 🚀 Adding a New Service
## Example Template
```
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.myapp.rule=Host(`myapp.aiserver.local`)"
  - "traefik.http.routers.myapp.entrypoints=web"
  - "traefik.http.services.myapp.loadbalancer.server.port=XXXX"
  - "traefik.docker.network=proxy"
```
Add network
```
networks:
  - default
  - proxy
```
## Add DNS rewrite

In AdGuard:
```
myapp.aiserver.local → 192.168.12.220
```
# 🔥 Example: Open WebUI
```
- "traefik.http.routers.ai.rule=Host(`ai.aiserver.local`)"
- "traefik.http.services.ai.loadbalancer.server.port=8080"
```
# ⚠️ Common Mistakes
## ❌ Missing proxy network

Result:

- Domain resolves
- Service does NOT load
## ❌ Wrong internal port

Result:

- 502 Bad Gateway
## ❌ Missing label

Result:

- Traefik ignores container
## ❌ Domain mismatch

Result:

- Traefik does not match request
# 🧠 Debugging Traefik
## Check containers
```
docker ps
```

## Check logs
```
docker logs traefik
```
## Check dashboard
```
http://192.168.12.220:8080
```
# 🔁 Mental Model
DNS → Traefik → Container
```
id="n4k2b7"
```
---

# 🚀 Summary

Traefik enables:

- Clean URLs
- No port usage
- Centralized routing
- Easy scalability

---

## 🧠 Rule of the system

``` id="t1x7p4"```
No label = No access
