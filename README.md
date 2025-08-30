# Dev Environment (`dev-env`)

> Local development environment for the **payments platform**, providing core infrastructure and orchestration via Docker Compose.  


## 1) Overview
This repository runs the supporting components required for all payment services. It allows developers to spin up the stack locally and interact with the platform through a unified API gateway.  

**Includes:**
- **Kong Gateway** — routes traffic to services (`/payments`, `/refunds`, etc.)  
- **Postgres** — relational database for services requiring persistence  
- **Redis** — in-memory store for idempotency keys and caching  
- **Service containers** — if repos are cloned locally, they build from source; otherwise prebuilt images are pulled  


## 2) Getting Started

### Prerequisites
- Docker & Docker Compose  
- Git  
- JDK 17+ (only if building services locally)  

### Clone
```bash
git clone https://github.com/<org>/dev-env.git
cd dev-env
cp .env.example .env
```

## 3) Running the Stack

### Start with prebuilt images (fast path)
```
docker compose up -d
```

### Start with local services (if repos cloned side-by-side)
```
docker compose -f docker-compose.yml -f docker-compose.local.yml up -d --build
```

### Stop
```
docker compose down -v
```

## 4) Access Points
- Gateway: http://localhost:8000
- Kong Admin: http://localhost:8001
- RedisInsight: http://localhost:5540
- Postgres: localhost:5432 (credentials from .env)

## 5) Project Layout
```
.
├── docker-compose.yml          # Base stack (infra + images)
├── docker-compose.local.yml    # Override for local builds
├── gateway/
│   └── kong.yml                # API gateway routes
├── .env.example                # Environment variables
└── Makefile                    # Helper commands
```

## 6) Common Commands
```
make up          # start stack
make up-local    # start with local builds
make down        # stop + remove volumes
make logs        # tail logs
make ps          # list containers
make reset-redis # flush redis state
```

## 7) Notes
- If a service repo (e.g., payment-service) is cloned at the same level as dev-env, Compose will build from local code.
- Otherwise, Compose pulls the prebuilt image from the registry.
- This setup makes onboarding fast while still supporting active development.

## 8) License
MIT
