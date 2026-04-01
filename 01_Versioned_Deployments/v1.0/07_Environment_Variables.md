# ⚙️ Jarvis Home AI Server — Environment Variables (v1.0)

---

## 🎯 Purpose

This document centralizes all environment variables used across services.

It ensures:

- Consistent configuration
- Easy updates
- Reproducible deployments

---

## 🧠 What Are Environment Variables?

Environment variables (`.env`) are used to:

- Store configuration values
- Avoid hardcoding sensitive data
- Simplify Docker Compose files

---

## 📁 Location

Main environment file:

```bash id="env_path_real"```
/docker/immich/.env
📦 Immich Environment Variables
UPLOAD_LOCATION=/data/photos/library

DB_DATA_LOCATION=/docker/immich/postgres

IMMICH_VERSION=release

DB_PASSWORD=immichpass123
DB_USERNAME=immich
DB_DATABASE_NAME=immich

DB_HOSTNAME=immich_postgres
REDIS_HOSTNAME=immich_redis
🔍 Variable Breakdown
📂 Storage
Variable	Description
UPLOAD_LOCATION	Where photos/videos are stored
DB_DATA_LOCATION	PostgreSQL data directory
🧠 Application
Variable	Description
IMMICH_VERSION	Docker image version
🗄️ Database
Variable	Description
DB_USERNAME	Database user
DB_PASSWORD	Database password
DB_DATABASE_NAME	Database name
DB_HOSTNAME	Database container
⚡ Redis
Variable	Description
REDIS_HOSTNAME	Redis container
🔐 Security Notes

⚠️ These values are sensitive:

DB_PASSWORD
🛡️ Recommendations
Do NOT commit .env to public GitHub
Use placeholders in documentation:
DB_PASSWORD=your_secure_password
🔁 How Docker Uses .env

In docker-compose.yml:

env_file:
  - .env

This injects variables into containers at runtime.

🧠 Best Practices
✅ Keep Variables Centralized
Use .env instead of hardcoding values
✅ Use Clear Naming
Example: DB_HOSTNAME instead of HOST
✅ Separate Concerns
Storage variables
Database variables
Service variables
✅ Version Control Strategy
File	Action
.env	❌ Do NOT commit
.env.example	✅ Commit
📄 Example .env.example
UPLOAD_LOCATION=/data/photos/library
DB_DATA_LOCATION=/docker/immich/postgres

IMMICH_VERSION=release

DB_PASSWORD=your_password
DB_USERNAME=immich
DB_DATABASE_NAME=immich

DB_HOSTNAME=immich_postgres
REDIS_HOSTNAME=immich_redis
🔄 Updating Variables

After editing .env:

docker compose down
docker compose up -d
⚠️ Common Issues
❌ Changes not applied

Cause:

Container not restarted

Fix:

Restart stack
❌ Wrong paths

Cause:

Incorrect mount path

Fix:

Verify /data and /docker paths
❌ Database connection errors

Cause:

Wrong credentials

Fix:

Check .env values match DB container
🧠 Mental Model
.env → docker-compose → container runtime
🚀 Summary

Environment variables allow:

Flexible configuration
Secure data handling
Easy system replication
Cleaner compose files
🔮 Future Improvements
Central .env for all services
Secret management (Vault / Docker secrets)
Per-service environment isolation
