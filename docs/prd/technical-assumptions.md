# Technical Assumptions

## Repository Structure: Monorepo

The project will use a monorepo structure containing:
- `/frontend` - React/TypeScript/Tailwind application
- `/backend` - Node.js/TypeScript API service
- `/infrastructure` - Docker Compose configurations, Makefiles, deployment scripts
- `/docs` - All project documentation

**Rationale:** A monorepo simplifies coordination between frontend and backend for this demonstration project, ensures version consistency, and provides a complete example in a single repository.

## Service Architecture

**Local Development:** Monolithic Docker Compose orchestration
- Frontend service (React dev server)
- Backend API service (Node.js/Express or Fastify)
- PostgreSQL database service
- Redis cache service

**Optional Cloud Deployment:** Kubernetes on GKE
- Each service containerized and deployed as separate pods
- Managed PostgreSQL and Redis (Cloud SQL, Memorystore)
- Ingress for routing

**Rationale:** Docker Compose provides the simplest local development experience while Kubernetes demonstrates production-ready deployment patterns. The architecture translates cleanly between environments.

## Technology Stack

**Frontend:**
- React 18+
- TypeScript
- Tailwind CSS
- Vite for build tooling

**Backend:**
- Node.js 20+
- TypeScript
- Express or Fastify framework
- Prisma ORM for database access
- Redis client for caching

**Database:**
- PostgreSQL 15+

**Cache:**
- Redis 7+

**Infrastructure:**
- Docker & Docker Compose for local development
- Makefile for command orchestration
- Optional: Kubernetes manifests for GKE deployment

**Rationale:** This stack represents modern, widely-adopted technologies with strong TypeScript support throughout. Docker Compose eliminates installation complexity. Makefile provides a simple, universal command interface.

## Testing Requirements

**Required:**
- Unit tests for backend business logic
- Integration tests for API endpoints
- Database migration tests

**Nice-to-have:**
- Frontend component tests
- End-to-end tests using Playwright or Cypress

**Testing Infrastructure:**
- Test database (separate Docker Compose profile or inline)
- Mock data generators
- CI/CD pipeline integration

**Rationale:** Testing is essential for production readiness. The MVP focuses on backend testing as it's most critical for data integrity. Frontend testing is valuable but can be added incrementally.

## Additional Technical Assumptions and Requests

- **Package Management:** npm or pnpm for JavaScript dependencies
- **Environment Variables:** `.env` files for local configuration (gitignored), with `.env.example` templates committed
- **Logging:** Structured logging (JSON format) with configurable log levels
- **API Documentation:** OpenAPI/Swagger for API endpoints
- **Git Hooks:** Optional pre-commit hooks for linting and formatting
- **Code Quality:** ESLint and Prettier configured for both frontend and backend
- **Hot Reload:** Both frontend and backend support hot reload during development
- **Port Assignments:** Standardized and documented (e.g., Frontend:3000, Backend:3001, PostgreSQL:5432, Redis:6379)
