# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

This is a full-stack FastAPI template with React frontend:

- **Backend**: FastAPI + SQLModel + PostgreSQL (in `/backend/`)
- **Frontend**: React + TypeScript + Vite + Chakra UI (in `/frontend/`)
- **Development**: Docker Compose with Traefik proxy for containerized development
- **Database**: PostgreSQL with Alembic migrations
- **Authentication**: JWT tokens with password hashing
- **Proxy**: Traefik v3.1 with self-signed certificates for HTTPS

## Development Commands

### Docker Compose (Recommended)
```bash
# Start full stack with HTTPS (main setup)
docker compose up -d

# Start with HTTP-only (development)
docker compose -f docker-compose.dev.yml up -d

# Stop services
docker compose down

# View logs
docker compose logs
docker compose logs backend frontend

# Rebuild specific service
docker compose build backend --no-cache
```

### Backend Development
```bash
cd backend

# Install dependencies
uv sync

# Run locally (requires database)
fastapi dev app/main.py

# Run tests with coverage
bash scripts/test.sh

# Format and lint
bash scripts/format.sh
bash scripts/lint.sh

# Type checking
mypy app

# Database migrations
alembic revision --autogenerate -m "description"
alembic upgrade head
```

### Frontend Development
```bash
cd frontend

# Install dependencies
npm install

# Start dev server
npm run dev

# Build for production
npm run build

# Lint and format
npm run lint

# Generate API client from backend OpenAPI
npm run generate-client

# End-to-end tests
npx playwright test
```

### Full Stack Testing
```bash
# Run all tests (builds containers, runs backend + frontend tests)
bash scripts/test.sh
```

## Key Architecture Patterns

### Backend Structure
- `app/api/routes/` - API endpoints organized by feature
- `app/models.py` - SQLModel database models
- `app/crud.py` - Database operations
- `app/core/` - Configuration, security, database setup
- `app/tests/` - Pytest test suite

### Frontend Structure
- `src/routes/` - TanStack Router file-based routing
- `src/components/` - Reusable UI components organized by feature
- `src/client/` - Auto-generated API client from backend OpenAPI
- `src/hooks/` - Custom React hooks
- `tests/` - Playwright E2E tests

### Authentication Flow
- JWT tokens stored in httpOnly cookies
- Protected routes use `useAuth` hook
- Backend validates tokens via dependency injection

## Development URLs

### HTTPS Setup (docker-compose.yml)
- Frontend: https://dashboard.eli.com
- Backend API: https://api.eli.com
- API Docs: https://api.eli.com/docs
- Database Admin: https://adminer.eli.com
- Traefik Dashboard: https://traefik.eli.com
- Database Direct: localhost:5433 (if port mapping enabled)

### HTTP-Only Setup (docker-compose.dev.yml)
- Frontend: http://dashboard.eli.com
- Backend API: http://api.eli.com
- API Docs: http://api.eli.com/docs
- Database Admin: http://adminer.eli.com
- Traefik Dashboard: http://traefik.eli.com

### Alternative Compose Files
- `docker-compose.traefik.yml` - Standalone Traefik with HTTPS
- `docker-compose.traefik-http.yml` - Standalone Traefik HTTP-only

## Environment Configuration
- No `.env` file required - all variables have defaults
- Override via environment variables (useful for GitHub Actions)
- Key variables: `DOMAIN`, `SECRET_KEY`, `POSTGRES_PASSWORD`
- Self-signed certificates in `/certs/` for HTTPS support
- Traefik network: `traefik-public` (created automatically)

## Certificates
- Self-signed certificate covers `*.eli.com`
- Browser will show security warning - click "Advanced" → "Proceed"
- Certificate files: `certs/eli.com.crt` and `certs/eli.com.key`