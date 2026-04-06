# CLAUDE.md

## Project

Paperclip — Self-hosted deployment via Docker with PostgreSQL and Cloudflare Tunnel.
Source: https://github.com/paperclipai/paperclip

## Build & Run

```bash
cp .env.example .env
# Vul .env in met je waarden
docker compose up --build -d
```

## Architecture

- `Dockerfile` — Multi-stage build: clones repo, installs deps, builds, creates production image
- `docker-compose.yaml` — 2 services: PostgreSQL 17, Paperclip server (localhost:3100)
- `docker-entrypoint.sh` — UID/GID matching voor volume permissions
- Server luistert alleen op 127.0.0.1:3100 — cloudflared connector op de host routeert het verkeer
