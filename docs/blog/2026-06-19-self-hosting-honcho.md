# Self-Hosting Honcho: A Memory Layer for AI Applications

*Posted on 2026-06-19*

**Honcho** is an open-source platform that provides a persistent memory layer for AI applications. It handles conversation history, document storage, semantic search with embeddings, and background reasoning via its Deriver worker.

In this post, I'll walk through how we self-hosted Honcho on our home-lab server (kubemaster) using Docker, with persistent PostgreSQL (pgvector) storage and Redis caching.

---

## Why Honcho?

As we build more AI-powered automation in the home lab, we need a persistent memory layer that can:

- Store conversation history and agent interactions
- Index documents with vector embeddings for semantic search
- Run background reasoning and memory consolidation
- Provide an API-first interface for applications to consume

---

## Architecture

Our Honcho deployment runs as three Docker containers:

| Service | Image | Port | Purpose |
|---------|-------|------|---------|
| **api** | Custom build (from source) | 8001 | FastAPI server |
| **database** | `pgvector/pgvector:pg15` | 5432 | PostgreSQL with pgvector |
| **redis** | `redis:8.2` | 6379 | Caching layer |

All ports are bound to localhost only.

---

## Storage Layout

All persistent data lives under `/data/honcho/`:

```
/data/honcho/
├── app/                      # Honcho source code + docker-compose.yml
├── postgres/                 # PostgreSQL data (pgvector)
└── redis/                    # Redis cache data
```

---

## Setup Steps

### Prerequisites

- Docker 29.0+ with Docker Compose v2
- An OpenAI-compatible API key (OpenAI, OpenRouter, Gemini, Ollama, etc.)
- At least 2GB free RAM and 5GB free disk

### 1. Clone and Configure

```bash
git clone https://github.com/plastic-labs/honcho.git /data/honcho/app
cd /data/honcho/app
cp .env.template .env
```

Edit `.env`:

```bash
# LLM API key (OpenAI or any compatible endpoint)
LLM_OPENAI_API_KEY=sk-your-api-key-here

# Disable auth for self-hosted
AUTH_USE_AUTH=*** Logging
LOG_LEVEL=INFO
```

### 2. Docker Compose

Create `docker-compose.yml`:

```yaml
services:
  api:
    build:
      context: .
      dockerfile: Dockerfile
    entrypoint: ["sh", "docker/entrypoint.sh"]
    depends_on:
      database:
        condition: service_healthy
      redis:
        condition: service_healthy
    ports:
      - "127.0.0.1:8001:8000"
    environment:
      - DB_CONNECTION_URI=postgresql+psycopg://postgres:postgres@database:5432/postgres
      - CACHE_URL=redis://redis:6379/0
      - CACHE_ENABLED=true
    env_file:
      - path: .env
        required: false
    restart: unless-stopped
    networks:
      - honcho-network

  database:
    image: pgvector/pgvector:pg15
    restart: unless-stopped
    command: ["postgres", "-c", "max_connections=200"]
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=***      - POSTGRES_HOST_AUTH_METHOD=***      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - /data/honcho/postgres:/var/lib/postgresql/data/
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres -d postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
    networks:
      - honcho-network

  redis:
    image: redis:8.2
    restart: unless-stopped
    volumes:
      - /data/honcho/redis:/data
    healthcheck:
      test: ["CMD-SHELL", "redis-cli ping"]
      interval: 5s
      timeout: 5s
      retries: 5
    networks:
      - honcho-network

networks:
  honcho-network:
    driver: bridge
```

### 3. Start Services

```bash
cd /data/honcho/app
docker compose up -d --build
```

The first build takes a few minutes (pulls images, installs Python deps, runs DB migrations).

### 4. Verify

```bash
# Check containers
docker compose ps

# Health check
curl http://localhost:8001/health
# {"status":"ok"}

# Smoke test
curl -X POST http://localhost:8001/v3/workspaces \
  -H "Content-Type: application/json" \
  -d '{"name":"test"}'
```

---

## Management Commands

```bash
docker compose start      # Start services
docker compose stop       # Stop services
docker compose restart    # Restart all
docker compose down       # Stop and remove containers (data persists)
docker compose logs --tail 50   # View logs
docker compose logs api --tail 20   # API-specific logs
```

### Updating

```bash
cd /data/honcho/app
git pull
docker compose up -d --build
```

---

## Connecting

```python
from honcho import Honcho

honcho = Honcho(base_url="http://192.168.1.18:8001")
peer = honcho.peer("my-agent")
session = peer.create_session()
```

---

## Key Points

- **Port 8001** — 8000 was already in use by another service
- **Persistent volumes** — data survives container rebuilds
- **LLM key required** — Deriver won't run without it
- **Auth disabled** — set `AUTH_USE_AUTH=*** for self-hosted

---

*Hosted on kubemaster (Rockchip RK3566, 8GB RAM, Armbian)*
*Source: [Honcho Self-Hosting Guide](https://honcho.dev/docs/v3/contributing/self-hosting)*
