# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

This is a full-stack FastAPI template with React frontend:

- **Backend**: FastAPI + SQLModel + PostgreSQL (in `/backend/`)
- **Frontend**: React + TypeScript + Vite + Chakra UI (in `/frontend/`)
- **Development**: Docker Compose for containerized development
- **Database**: PostgreSQL with Alembic migrations
- **Authentication**: JWT tokens with password hashing

## Development Commands

### Docker Compose (Recommended)
```bash
# Start full stack with hot reload
docker compose watch

# Stop specific services
docker compose stop frontend
docker compose stop backend

# View logs
docker compose logs
docker compose logs backend
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

### With Custom Domain (Traefik Proxy)
- Frontend: http://eli.com
- Backend: http://api.eli.com
- API Docs: http://api.eli.com/docs

### Direct Access (Default Ports)
- Frontend: http://localhost:5173
- Backend: http://localhost:8000
- API Docs: http://localhost:8000/docs
- Database Admin: http://localhost:8080

## Environment Configuration
- `.env` files control Docker Compose behavior
- Backend environment in `backend/.env`
- Frontend environment in `frontend/.env`
- Change `SECRET_KEY`, `FIRST_SUPERUSER_PASSWORD`, `POSTGRES_PASSWORD` before deployment