# FYNO Private Home AI Architecture

This document describes the architecture of the **Jarvis Home AI Server**, a self-hosted platform designed to provide:

- AI assistance
- personal data ownership
- visual memory
- document intelligence
- media services
- automation

The system is built around **Docker containers**, a **reverse proxy**, and **local AI models**.

---

# System Overview

Jarvis is designed as a layered architecture.

                ┌──────────────────────────┐
                │        User Devices       │
                │  PC • Phone • Tablet     │
                └─────────────┬────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │     Traefik      │
                    │ Reverse Proxy    │
                    └────────┬────────┘
                             │
    ┌────────────────────────┼─────────────────────────┐
    ▼                        ▼                         ▼

┌───────────────┐ ┌────────────────┐ ┌─────────────────┐
│ Immich │ │ Nextcloud │ │ Jellyfin │
│ Photo AI │ │ File Cloud │ │ Media Server │
└───────────────┘ └────────────────┘ └─────────────────┘

    ▼                        ▼                         ▼

            ┌───────────────────────────────────┐
            │         Jarvis AI Layer           │
            │                                   │
            │  Ollama (Local LLMs)              │
            │  OpenWebUI (Chat Interface)       │
            │  Whisper (Speech-to-Text)         │
            │  Piper / TTS (Voice Output)       │
            │                                   │
            └───────────────────────────────────┘

                            ▼

                ┌─────────────────────────┐
                │      Data Storage       │
                │                         │
                │  /data/photos           │
                │  /data/documents        │
                │  /data/media            │
                │  /data/cloud            │
                │                         │
                └─────────────────────────┘

---

# Infrastructure Components

## Reverse Proxy

**Traefik**

Purpose:

- service routing
- hostname-based access
- container auto-discovery

Example routes:


immich.aiserver.local
cloud.aiserver.local
jellyfin.aiserver.local
jarvis.aiserver.local


---

## Container Management

**Docker**

All services run as isolated containers.

Benefits:

- easy upgrades
- service isolation
- portability

---

## Container UI

**Portainer**

Provides:

- container monitoring
- container logs
- service management

---

# AI Layer

Jarvis will run fully local AI models.

Components planned:

| Component | Purpose |
|--------|--------|
| Ollama | Run local LLMs |
| Open WebUI | Chat interface |
| Whisper | Speech recognition |
| Piper | Voice synthesis |

---

# Visual Memory System

**Immich**

Provides:

- photo storage
- face recognition
- object detection
- visual search

Storage path:


/data/photos/library


Example queries Jarvis will support:


Show photos from last Christmas
Find pictures of the dog
Where was this photo taken?


---

# Document Intelligence

Planned system:

**Paperless-ngx**

Features:

- document scanning
- OCR
- AI searchable archive

Storage:


/data/documents


---

# Personal Cloud

Planned system:

**Nextcloud**

Features:

- file sync
- calendar
- contacts
- shared documents

Storage:


/data/cloud


---

# Media System

Planned system:

**Jellyfin**

Features:

- movie streaming
- TV shows
- music
- audiobook library

Storage:


/data/media


---

# Storage Layout

Server storage structure:


/data
│
├── photos
│ └── library
│
├── documents
│
├── media
│
└── cloud


All services store data under `/data` for centralized backup.

---

# Network Architecture

Internal hostname routing:


immich.aiserver.local
jellyfin.aiserver.local
cloud.aiserver.local
jarvis.aiserver.local


Resolved through:

- local DNS
- hosts file entries

Example:


192.168.12.220 immich.aiserver.local


---

# Hardware

Server hardware:

| Component | Spec |
|--------|--------|
CPU | Multi-core server CPU |
RAM | 32GB |
GPU | RTX 3060 (planned) |
Storage | RAID array |

---

# Future Expansion

Planned capabilities:

- voice assistant
- home automation
- local AI agents
- knowledge graph
- smart search across all personal data

---

# Project Goal

The objective of this project is to build a **fully self-hosted personal AI system** that replaces cloud services while keeping all data private.
