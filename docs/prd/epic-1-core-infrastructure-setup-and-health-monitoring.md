# Epic 1: Core Infrastructure Setup & Health Monitoring

**Goal:** Establish the foundational project structure, containerization, and service orchestration to deliver a working multi-service environment that can be started with a single command and verified as healthy.

## Story 1.1: Project Initialization and Repository Structure

**As a** developer,
**I want** a well-organized monorepo with clear separation of concerns,
**so that** I can easily navigate and understand the project structure.

**Acceptance Criteria:**
1. Repository contains `/frontend`, `/backend`, `/infrastructure`, and `/docs` directories
2. Root directory includes `README.md`, `.gitignore`, and `Makefile`
3. `.gitignore` excludes `node_modules`, `.env`, and build artifacts
4. README includes project overview and quick start instructions
5. Each service directory contains its own `README.md` with service-specific information

## Story 1.2: Docker Compose Configuration for All Services

**As a** developer,
**I want** a Docker Compose configuration that defines all services,
**so that** I can run the entire stack with Docker Compose commands.

**Acceptance Criteria:**
1. `docker-compose.yml` defines services for frontend, backend, PostgreSQL, and Redis
2. Services are configured with appropriate networks for inter-service communication
3. PostgreSQL and Redis use official Docker images with appropriate versions
4. Volume mounts are configured for database persistence and code hot-reload
5. Environment variables are externalized to `.env` file (with `.env.example` provided)
6. Service dependency ordering is specified (backend depends on database and Redis)

## Story 1.3: Makefile Commands for Environment Management

**As a** developer,
**I want** simple make commands to control my environment,
**so that** I don't need to remember complex Docker Compose commands.

**Acceptance Criteria:**
1. `make dev` command starts all services in the correct order
2. `make down` command stops and removes all containers
3. `make logs` command tails logs from all services
4. `make clean` command removes all volumes and performs full cleanup
5. Makefile includes help text documenting all available commands
6. Commands provide clear feedback about what they're doing

## Story 1.4: Backend Service with Health Check Endpoint

**As a** developer,
**I want** a backend API service with a health check endpoint,
**so that** I can verify the service is running and ready to handle requests.

**Acceptance Criteria:**
1. Node.js/TypeScript backend service runs in a Docker container
2. `/health` endpoint returns 200 status and JSON response `{"status": "healthy"}`
3. Health check verifies database connection is active
4. Health check verifies Redis connection is active
5. Dockerfile uses multi-stage build for optimized image size
6. Hot reload is enabled for development (code changes trigger restart)
7. TypeScript is properly configured with `tsconfig.json`

## Story 1.5: Frontend Service with Development Server

**As a** developer,
**I want** a React frontend service with hot reload,
**so that** I can develop UI features with immediate feedback.

**Acceptance Criteria:**
1. React/TypeScript/Tailwind frontend runs in a Docker container
2. Development server is accessible at `http://localhost:3000`
3. Hot reload works when source files are modified
4. Frontend displays a welcome page confirming the service is running
5. Tailwind CSS is properly configured and working
6. Vite build tooling is configured for optimal development experience

## Story 1.6: Environment Startup Verification Script

**As a** developer,
**I want** automated verification that all services are healthy after startup,
**so that** I know my environment is ready for development.

**Acceptance Criteria:**
1. Startup script polls all service health endpoints
2. Script waits up to 60 seconds for services to become healthy
3. Clear progress indicators show which services are ready
4. Script exits with success when all services are healthy
5. Script exits with failure and helpful error message if any service fails
6. `make dev` includes this verification as final step
