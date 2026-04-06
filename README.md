<p align="center">
  <img src="https://raw.githubusercontent.com/paperclipai/paperclip/master/doc/assets/header.png" alt="Paperclip" width="500" />
  <br />
  <img src="https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png" alt="Docker" width="80" />
</p>

<h1 align="center">🐳 Paperclip Docker</h1>

<p align="center">
  <strong>Self-host Paperclip on your own server — built from source, Dockerized, ready to go.</strong>
</p>

<p align="center">
  <a href="https://paperclip.ing"><img src="https://img.shields.io/badge/🌐%20paperclip.ing-official%20site-8B5CF6.svg" alt="Official Site" /></a>
  <a href="https://github.com/paperclipai/paperclip"><img src="https://img.shields.io/badge/📦%20upstream-paperclipai%2Fpaperclip-6D28D9.svg?logo=github&logoColor=white" alt="Upstream Repo" /></a>
  <a href="https://github.com/MadeByAdem/paperclip/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License" /></a>
  <img src="https://img.shields.io/badge/docker-compose%20v2-2496ED.svg?logo=docker&logoColor=white" alt="Docker Compose" />
  <a href="https://github.com/paperclipai/paperclip"><img src="https://img.shields.io/github/stars/paperclipai/paperclip?style=flat&label=⭐%20paperclip%20stars" alt="Stars" /></a>
</p>

---

> [!WARNING]
> ⚠️ This is an **unofficial, community-maintained** Docker setup. It is **not affiliated with, endorsed by, or supported by** the Paperclip team or [paperclipai](https://github.com/paperclipai). Use at your own risk. For official support, refer to the [upstream repository](https://github.com/paperclipai/paperclip).

> **What is Paperclip?** Paperclip is an open-source orchestration platform for autonomous AI companies. If OpenClaw is an *employee*, Paperclip is the *company*. This repository lets you self-host it in Docker — built from source with every tool included, reverse-proxy ready, zero ports exposed.

---

## 📋 Table of Contents

- [📦 What's Included](#-whats-included)
- [✅ Prerequisites](#-prerequisites)
- [🚀 Installation](#-installation)
- [🏗 Architecture](#-architecture)
- [🌐 Connecting a Reverse Proxy](#-connecting-a-reverse-proxy)
- [🔐 Environment Variables](#-environment-variables)
- [⚙️ Managing Your Server](#️-managing-your-server)
- [💾 Data & Backups](#-data--backups)
- [🔄 Updating](#-updating)
- [🔧 Troubleshooting](#-troubleshooting)
- [📁 File Overview](#-file-overview)
- [📄 License](#-license)

---

## 📦 What's Included

This project adds a production-ready Docker layer on top of the official Paperclip source:

| | Feature |
| --- | --- |
| 🏗️ | **Full Paperclip build from source** — UI, server, database, all agent adapters |
| 🤖 | **Every CLI tool pre-installed** — Claude Code, Codex, opencode-ai, GitHub CLI, ripgrep |
| 🔒 | **Binds to `127.0.0.1` only** — designed for Cloudflare Tunnel or any reverse proxy |
| 🔑 | **UID/GID matching entrypoint** — seamless volume permissions, no root needed |

<div align="center">
<table>
  <tr>
    <td align="center"><strong>Included<br/>tools</strong></td>
    <td align="center">🟣<br/><sub>Claude Code</sub></td>
    <td align="center">🟢<br/><sub>Codex</sub></td>
    <td align="center">🔵<br/><sub>opencode-ai</sub></td>
    <td align="center">⚙️<br/><sub>GitHub CLI</sub></td>
    <td align="center">🔍<br/><sub>ripgrep</sub></td>
    <td align="center">🐍<br/><sub>Python 3</sub></td>
  </tr>
</table>
</div>

Everything else is standard Paperclip — no custom code.

---

## ✅ Prerequisites

Before you start, make sure you have:

| # | Requirement | Details |
| - | ----------- | ------- |
| 1 | 🖥️ **Linux server** | Ubuntu recommended, any Docker-capable OS works |
| 2 | 🐳 **Docker Engine + Compose v2** | [Install Docker](https://docs.docker.com/engine/install/) |
| 3 | 🌐 **A reverse proxy** | Cloudflare Tunnel, Nginx, Caddy, Traefik, etc. |

<details>
<summary>📥 <strong>Installing Docker (if you don't have it yet)</strong></summary>

<br/>

```bash
# Install Docker
curl -fsSL https://get.docker.com | sudo sh

# Allow your user to run Docker without sudo
sudo usermod -aG docker $USER

# Log out and back in for the group change to take effect
exit
```

After logging back in, verify it works:

```bash
docker --version
docker compose version
```

Both commands should print a version number without errors.

</details>

---

## 🚀 Installation

### Option A — ⚡ Automated setup (recommended)

```bash
git clone https://github.com/MadeByAdem/paperclip.git
cd paperclip
chmod +x setup.sh
./setup.sh
```

The setup script will automatically:

1. ✅ Check that Docker and Docker Compose are installed
2. 🔑 Generate a `.env` file with secure secrets
3. 🐳 Build the Docker image from source
4. 🚀 Start Paperclip and PostgreSQL

After setup, edit `.env` and set `PAPERCLIP_PUBLIC_URL` to your domain.

### Option B — 🔧 Manual setup

<details>
<summary>Click to expand manual steps</summary>

<br/>

#### Step 1 — 📥 Clone this repository

```bash
git clone https://github.com/MadeByAdem/paperclip.git
cd paperclip
```

#### Step 2 — 🔧 Configure environment

```bash
cp .env.example .env
```

Generate and paste your auth secret:

```bash
openssl rand -hex 32
```

Edit `.env` and fill in:

| Variable | What to do |
| -------- | ---------- |
| 🔑 `BETTER_AUTH_SECRET` | Paste the generated secret |
| 🌍 `PAPERCLIP_PUBLIC_URL` | Set to your domain (e.g. `https://paperclip.example.com`) |

#### Step 3 — 🐳 Deploy

```bash
docker compose up -d --build
```

> [!NOTE]
> ☕ The first build clones and compiles Paperclip from source — this takes a few minutes.

#### Step 4 — ✅ Verify it's running

```bash
docker compose logs -f server
```

You should see log output indicating the server is active. Press `Ctrl+C` to stop following the logs (the server keeps running in the background).

<br/>
</details>

### 🎬 First Run — Instance Setup

After the containers are running, you need to onboard your Paperclip instance and create the first admin account.

#### Step 1 — Run the onboard wizard

```bash
docker exec -it paperclipai-server-1 pnpm paperclipai onboard
```

Choose **Advanced setup** and use these recommended values:

| Prompt | Recommended value |
| ------ | ----------------- |
| Database mode | Embedded PostgreSQL (managed locally) |
| Data directory | `/paperclip/instances/default/db` |
| PostgreSQL port | `54329` |
| Automatic backups | Yes |
| Backup directory | `/paperclip/instances/default/data/backups` |
| Backup interval | `60` minutes |
| Backup retention | `30` days |
| LLM provider | Claude (Anthropic) — or your preferred provider |
| Logging mode | File-based logging |
| Log directory | `/paperclip/instances/default/logs` |
| Deployment mode | Authenticated |
| Exposure profile | Private network |
| Bind host | `0.0.0.0` |
| Server port | `3100` |
| Allowed hostnames | Your domain (e.g. `paperclip.example.com`) |
| Storage provider | Local disk |
| Storage directory | `/paperclip/instances/default/data/storage` |

When prompted to start Paperclip, choose **Yes**.

#### Step 2 — Create the first admin account

```bash
docker exec -it paperclipai-server-1 pnpm paperclipai auth bootstrap-ceo
```

This generates an invite URL. Open it in your browser to create your admin account.

> [!NOTE]
> After onboarding, restart the server to pick up the new config:
>
> ```bash
> docker compose restart server
> ```

### 🌐 Next step — Connect a reverse proxy

See [🌐 Connecting a Reverse Proxy](#-connecting-a-reverse-proxy) below.

---

## 🏗 Architecture

```
                          your-domain.com
                                │
┌───────────────────────────────┼──────────────────────────────────┐
│  Your Server                  │                                  │
│                    ┌──────────▼───────────┐                      │
│                    │   Reverse Proxy       │                      │
│                    │   cloudflared / nginx │                      │
│                    └──────────┬───────────┘                      │
│                               │                                  │
│              ┌────────────────▼────────────────┐                 │
│              │         Docker Compose          │                 │
│              │                                 │                 │
│              │  ┌───────────┐  ┌────────────┐  │                 │
│              │  │ Paperclip │◄►│ PostgreSQL  │  │                 │
│              │  │  :3100    │  │    17       │  │                 │
│              │  └───────────┘  └────────────┘  │                 │
│              └─────────────────────────────────┘                 │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

> 🔒 **Port `3100` binds to `127.0.0.1` only.** It is never exposed to the internet. Your existing reverse proxy or Cloudflare Tunnel routes public traffic to it.

---

## 🌐 Connecting a Reverse Proxy

<details>
<summary>☁️ <strong>Cloudflare Tunnel</strong> — recommended for zero-port setups</summary>

<br/>

In your **Cloudflare Zero Trust** dashboard, add a public hostname to your existing tunnel:

| Field | Value |
| ----- | ----- |
| Subdomain | `paperclip` |
| Domain | `example.com` |
| Service Type | `HTTP` |
| URL | `localhost:3100` |

Your `cloudflared` connector picks it up automatically — no restart needed.

<br/>
</details>

<details>
<summary>🔷 <strong>Nginx</strong></summary>

<br/>

```nginx
server {
    listen 443 ssl;
    server_name paperclip.example.com;

    ssl_certificate     /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://127.0.0.1:3100;
        proxy_http_version 1.1;

        # Required for WebSocket support
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

<br/>
</details>

---

## 🔐 Environment Variables

| Variable | Required | Default | Description |
| -------- | :------: | ------- | ----------- |
| 🔑 `BETTER_AUTH_SECRET` | ✅ | — | Auth secret. Generate with `openssl rand -hex 32` |
| 🌍 `PAPERCLIP_PUBLIC_URL` | ✅ | — | Public URL (e.g. `https://paperclip.example.com`) |
| 🗄️ `POSTGRES_PASSWORD` | | `paperclip` | PostgreSQL password |
| 🟣 `ANTHROPIC_API_KEY` | | — | API key for Claude agent adapter |
| 🟢 `OPENAI_API_KEY` | | — | API key for Codex agent adapter |

---

## ⚙️ Managing Your Server

Common commands for managing your Paperclip instance:

| Action | Command |
| ------ | ------- |
| 📋 View live logs | `docker compose logs -f server` |
| ⏹️ Stop the server | `docker compose down` |
| ▶️ Start the server | `docker compose up -d` |
| 🔄 Restart the server | `docker compose restart server` |
| 🔨 Rebuild after updates | `docker compose down && docker compose build --no-cache && docker compose up -d` |

---

## 💾 Data & Backups

All data lives in Docker volumes — survives restarts, `down`, and rebuilds.

| Volume | Container Path | Contents |
| ------ | -------------- | -------- |
| 🗄️ `pgdata` | `/var/lib/postgresql/data` | PostgreSQL database |
| 📂 `paperclip-data` | `/paperclip` | Config, agent workspaces, instance data |

<details>
<summary>📥 <strong>Backup</strong></summary>

<br/>

```bash
# Database
docker compose exec db pg_dump -U paperclip paperclip > backup.sql

# Application data
docker run --rm -v paperclip-docker_paperclip-data:/data -v $(pwd):/backup \
  alpine tar czf /backup/paperclip-data.tar.gz -C /data .
```

<br/>
</details>

<details>
<summary>📤 <strong>Restore</strong></summary>

<br/>

```bash
# Database
docker compose exec -T db psql -U paperclip paperclip < backup.sql

# Application data
docker run --rm -v paperclip-docker_paperclip-data:/data -v $(pwd):/backup \
  alpine tar xzf /backup/paperclip-data.tar.gz -C /data
```

<br/>
</details>

---

## 🔄 Updating

```bash
docker compose down
docker compose build --no-cache
docker compose up -d
```

The Dockerfile clones the latest Paperclip source on each build, so `--no-cache` pulls the newest version.

---

## 🔧 Troubleshooting

<details>
<summary>❌ <strong>"BETTER_AUTH_SECRET must be set"</strong></summary>

<br/>

Generate and add the secret to your `.env`:

```bash
echo "BETTER_AUTH_SECRET=$(openssl rand -hex 32)" >> .env
```

<br/>
</details>

<details>
<summary>🔒 <strong>Permission errors on the data volume</strong></summary>

<br/>

The entrypoint auto-matches container UID/GID to your host. If it still fails, rebuild with explicit IDs:

```bash
docker compose build --build-arg USER_UID=$(id -u) --build-arg USER_GID=$(id -g)
```

<br/>
</details>

<details>
<summary>🔌 <strong>WebSocket errors through reverse proxy</strong></summary>

<br/>

Paperclip uses WebSockets for real-time updates. Your reverse proxy must forward the `Upgrade` and `Connection` headers. See the [Nginx config](#-connecting-a-reverse-proxy) above.

<br/>
</details>

<details>
<summary>🏗 <strong>Build fails at pnpm install</strong></summary>

<br/>

The upstream lockfile may have changed. Force a fresh build:

```bash
docker compose build --no-cache
```

<br/>
</details>

---

## 📁 File Overview

| File | Purpose |
| ---- | ------- |
| 🐳 `Dockerfile` | Multi-stage build (clone → deps → build → production) |
| 📦 `docker-compose.yaml` | PostgreSQL 17 + Paperclip server |
| 🔑 `docker-entrypoint.sh` | UID/GID matching for volume permissions |
| ⚙️ `.env.example` | Environment variable template |
| 🚀 `setup.sh` | Automated setup script for beginners |
| 🛡️ `.dockerignore` | Keeps secrets and docs out of the build context |

---

## 📄 License

MIT &copy; 2025 [MadeByAdem](https://github.com/MadeByAdem)

Paperclip itself is maintained by [paperclipai](https://github.com/paperclipai/paperclip) under its own [MIT license](https://github.com/paperclipai/paperclip/blob/master/LICENSE).
