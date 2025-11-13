# Zero-to-Running Developer Environment Product Requirements Document (PRD)

**Organization:** Wander
**Project ID:** 3MCcAvCyK7F77BpbXUSI_1762376408364
**Version:** 1.0
**Last Updated:** 2025-01-13

---

## Goals and Background Context

### Goals

- Minimize time spent on environment setup and management for developers
- Enable new developers to start coding within 10 minutes of cloning the repository
- Eliminate "works on my machine" problems through consistent, reproducible environments
- Reduce environment-related support tickets by 90%
- Provide a template for modern, containerized local development workflows
- Demonstrate best practices for secret management, service orchestration, and health monitoring

### Background Context

Developers consistently face significant delays and frustrations when setting up local development environments. Complex multi-service architectures requiring databases, caches, APIs, and frontends create a maze of installation steps, version conflicts, and configuration challenges. This problem compounds when onboarding new team members or switching between projects, leading to hours or days of non-productive troubleshooting.

The Zero-to-Running Developer Environment addresses this by providing a single-command orchestrated setup that brings up a complete, healthy, multi-service development stack. By leveraging Docker Compose for local development and providing clear pathways to cloud deployment, this solution enables developers to focus on writing code rather than managing infrastructure.

### Change Log

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2025-01-13 | 1.0 | Initial PRD creation | Product Manager |

---

## Requirements

### Functional

- **FR1:** The system shall provide a single command (`make dev`) that brings up the entire development stack including frontend, backend API, PostgreSQL database, and Redis cache
- **FR2:** The system shall verify all services are healthy and communicating before completing the startup process
- **FR3:** The system shall provide a single command (`make down`) that cleanly tears down the entire environment
- **FR4:** The system shall support externalized configuration through environment variables and configuration files
- **FR5:** The system shall demonstrate secure handling of mock secrets with patterns applicable to real secret management
- **FR6:** The system shall enable inter-service communication with the API able to access the database and cache
- **FR7:** The system shall include health check endpoints for all services
- **FR8:** The system shall provide comprehensive documentation for onboarding new developers
- **FR9:** The system shall support automatic service dependency ordering (database starts before API)
- **FR10:** The system shall provide meaningful output and logging during the startup process
- **FR11:** The system shall include developer-friendly defaults such as hot reload for code changes
- **FR12:** The system shall expose debug ports for backend services
- **FR13:** The system shall handle common errors gracefully (port conflicts, missing dependencies)
- **FR14:** The system shall support optional database seeding with test data
- **FR15:** The system shall include optional deployment documentation for GKE (Google Kubernetes Engine)

### Non Functional

- **NFR1:** The environment setup process shall complete in under 10 minutes on a standard development machine
- **NFR2:** The system shall use industry-standard tools and avoid custom dependencies where possible
- **NFR3:** All services shall restart automatically if they crash during development
- **NFR4:** The system shall provide clear, actionable error messages when failures occur
- **NFR5:** The solution shall be maintainable by developers with basic Docker and command-line knowledge
- **NFR6:** The system shall minimize resource usage to enable running on typical developer laptops
- **NFR7:** All secrets shall be externalized and never committed to version control
- **NFR8:** The system shall support parallel service startup where feasible to optimize boot time

---

## User Interface Design Goals

### Overall UX Vision

This is a developer tool accessed primarily through command-line interface. The UX focuses on clarity, simplicity, and informative feedback. Developers should feel confident that the system is working correctly at every step, with clear progress indicators and actionable error messages when issues arise.

### Key Interaction Paradigms

- **Command-Line First:** All interactions occur through simple make commands
- **Feedback-Driven:** Continuous status updates during startup/shutdown processes
- **Self-Service:** Developers should be able to diagnose and fix common issues without external help
- **Documentation-Integrated:** Help and guidance available directly in README and command output

### Core Screens and Views

- **Terminal Output during `make dev`:** Shows service startup sequence, health checks, and final success message
- **Terminal Output during `make down`:** Shows graceful shutdown process
- **README Documentation:** Primary onboarding and reference document
- **Service Logs:** Accessible through Docker Compose for debugging
- **Optional Web Dashboard:** Browser-based view of running services and health status

### Accessibility

Not applicable - command-line tool with standard terminal accessibility support

### Branding

Clean, professional terminal output with minimal use of color for status indicators (green for success, yellow for warnings, red for errors). Documentation follows standard markdown conventions.

### Target Device and Platforms

- **Primary:** macOS and Linux developer workstations
- **Secondary:** Windows with WSL2 support
- All platforms must support Docker Desktop or equivalent container runtime

---

## Technical Assumptions

### Repository Structure: Monorepo

The project will use a monorepo structure containing:
- `/frontend` - React/TypeScript/Tailwind application
- `/backend` - Node.js/TypeScript API service
- `/infrastructure` - Docker Compose configurations, Makefiles, deployment scripts
- `/docs` - All project documentation

**Rationale:** A monorepo simplifies coordination between frontend and backend for this demonstration project, ensures version consistency, and provides a complete example in a single repository.

### Service Architecture

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

### Technology Stack

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

### Testing Requirements

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

### Additional Technical Assumptions and Requests

- **Package Management:** npm or pnpm for JavaScript dependencies
- **Environment Variables:** `.env` files for local configuration (gitignored), with `.env.example` templates committed
- **Logging:** Structured logging (JSON format) with configurable log levels
- **API Documentation:** OpenAPI/Swagger for API endpoints
- **Git Hooks:** Optional pre-commit hooks for linting and formatting
- **Code Quality:** ESLint and Prettier configured for both frontend and backend
- **Hot Reload:** Both frontend and backend support hot reload during development
- **Port Assignments:** Standardized and documented (e.g., Frontend:3000, Backend:3001, PostgreSQL:5432, Redis:6379)

---

## Epic List

### Epic 1: Core Infrastructure Setup & Health Monitoring
Establish foundational project structure, containerization, and service orchestration with health checks to deliver a working multi-service environment.

### Epic 2: Service Integration & Data Flow
Implement database schema, API endpoints, and frontend components that demonstrate end-to-end data flow from UI through API to database and cache.

### Epic 3: Developer Experience & Documentation
Enhance developer workflows with comprehensive documentation, debugging tools, error handling, and optional features like database seeding.

### Epic 4 (Optional): Advanced Features & Cloud Deployment
Provide optional Kubernetes deployment configurations, monitoring, and advanced features like multiple environment profiles and performance optimizations.

---

## Epic 1: Core Infrastructure Setup & Health Monitoring

**Goal:** Establish the foundational project structure, containerization, and service orchestration to deliver a working multi-service environment that can be started with a single command and verified as healthy.

### Story 1.1: Project Initialization and Repository Structure

**As a** developer,
**I want** a well-organized monorepo with clear separation of concerns,
**so that** I can easily navigate and understand the project structure.

**Acceptance Criteria:**
1. Repository contains `/frontend`, `/backend`, `/infrastructure`, and `/docs` directories
2. Root directory includes `README.md`, `.gitignore`, and `Makefile`
3. `.gitignore` excludes `node_modules`, `.env`, and build artifacts
4. README includes project overview and quick start instructions
5. Each service directory contains its own `README.md` with service-specific information

### Story 1.2: Docker Compose Configuration for All Services

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

### Story 1.3: Makefile Commands for Environment Management

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

### Story 1.4: Backend Service with Health Check Endpoint

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

### Story 1.5: Frontend Service with Development Server

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

### Story 1.6: Environment Startup Verification Script

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

---

## Epic 2: Service Integration & Data Flow

**Goal:** Implement database schema, API endpoints, and frontend components that demonstrate end-to-end data flow from UI through API to database and cache, proving inter-service communication works correctly.

### Story 2.1: Database Schema and Migrations

**As a** developer,
**I want** a database schema with migration tooling,
**so that** I can define and evolve the data model in a controlled way.

**Acceptance Criteria:**
1. Prisma ORM is configured with PostgreSQL connection
2. Initial schema defines at least one entity (e.g., `User` or `Item`)
3. Database migrations can be run with `npm run migrate`
4. Schema includes appropriate indexes for common queries
5. Migration scripts are version-controlled and reproducible
6. README documents how to create and run migrations

### Story 2.2: Backend API CRUD Endpoints

**As a** developer,
**I want** RESTful API endpoints for basic CRUD operations,
**so that** I can interact with the database through the API.

**Acceptance Criteria:**
1. POST endpoint creates new records in the database
2. GET endpoint retrieves records (single and list)
3. PUT/PATCH endpoint updates existing records
4. DELETE endpoint removes records
5. All endpoints return appropriate HTTP status codes
6. Request/response bodies use JSON format
7. API includes basic error handling and validation
8. OpenAPI/Swagger documentation is generated for all endpoints

### Story 2.3: Redis Caching Layer

**As a** developer,
**I want** Redis caching for frequently accessed data,
**so that** I can improve API performance and demonstrate cache integration.

**Acceptance Criteria:**
1. GET requests check Redis cache before querying database
2. Cache hits return data from Redis with appropriate headers
3. POST/PUT/DELETE operations invalidate relevant cache entries
4. Cache TTL is configurable via environment variables
5. Cache keys follow a consistent naming convention
6. Logging indicates cache hits vs. misses for debugging

### Story 2.4: Frontend API Integration

**As a** developer,
**I want** the frontend to fetch and display data from the backend API,
**so that** I can demonstrate full-stack integration.

**Acceptance Criteria:**
1. Frontend makes HTTP requests to backend API endpoints
2. API base URL is configurable via environment variables
3. UI displays a list of items fetched from the API
4. Loading states are shown while data is being fetched
5. Error states are displayed if API requests fail
6. Environment variables differentiate local vs. production API URLs

### Story 2.5: Frontend Create/Update Form

**As a** developer,
**I want** a form to create and update records through the API,
**so that** I can demonstrate full CRUD operations from the UI.

**Acceptance Criteria:**
1. Form includes appropriate input fields for the data model
2. Form validation prevents submission of invalid data
3. Successful submissions show confirmation message
4. Failed submissions display error messages
5. After successful create/update, the list view refreshes to show changes
6. Form includes cancel action that discards changes

---

## Epic 3: Developer Experience & Documentation

**Goal:** Enhance developer workflows with comprehensive documentation, debugging tools, error handling, and optional features like database seeding to ensure developers can be productive immediately.

### Story 3.1: Comprehensive README Documentation

**As a** new developer,
**I want** clear, comprehensive documentation,
**so that** I can set up and use the environment without external help.

**Acceptance Criteria:**
1. README includes prerequisites (Docker, Node.js versions)
2. Quick start section guides from clone to running in minimal steps
3. Available make commands are documented with descriptions
4. Architecture diagram shows service relationships
5. Troubleshooting section addresses common issues (port conflicts, Docker not running)
6. Environment variables are documented with descriptions and example values
7. Links to additional documentation in `/docs` are provided

### Story 3.2: Database Seeding with Test Data

**As a** developer,
**I want** to optionally seed the database with test data,
**so that** I can immediately test the application with realistic data.

**Acceptance Criteria:**
1. `make seed` command populates database with sample records
2. Seed data is realistic and representative of production data
3. Seeding is idempotent (running twice doesn't create duplicates)
4. Seed script includes documentation of what data is created
5. Optional: `make seed-clean` removes all seeded data
6. README documents seeding commands and when to use them

### Story 3.3: Enhanced Error Handling and Logging

**As a** developer,
**I want** clear error messages and structured logging,
**so that** I can quickly diagnose issues during development.

**Acceptance Criteria:**
1. Backend uses structured JSON logging with configurable log levels
2. All API errors return consistent JSON error format with error codes
3. Validation errors include field-specific messages
4. Database connection errors are logged with retry information
5. Frontend displays user-friendly error messages for API failures
6. Docker Compose logs are properly labeled by service name
7. README documents how to adjust log levels

### Story 3.4: Development Tools and Debugging Setup

**As a** developer,
**I want** debugging tools and utilities configured,
**so that** I can efficiently troubleshoot issues.

**Acceptance Criteria:**
1. Backend exposes debug port for Node.js debugging (e.g., 9229)
2. VSCode launch configuration is provided for backend debugging
3. `make logs` supports filtering by service name
4. `make shell-backend` command opens shell in backend container
5. `make db-cli` command opens PostgreSQL CLI for direct database access
6. ESLint and Prettier are configured for both frontend and backend
7. README documents debugging approaches

### Story 3.5: Secret Management Documentation and Patterns

**As a** developer,
**I want** clear patterns for managing secrets,
**so that** I can keep sensitive configuration secure.

**Acceptance Criteria:**
1. `.env.example` file documents all required environment variables
2. `.env` is gitignored and never committed
3. README explains how to create `.env` from `.env.example`
4. Documentation explains the difference between development and production secret management
5. Mock secrets used in development are clearly marked as non-production
6. Instructions provided for using secret management tools in production (e.g., Kubernetes secrets, cloud secret managers)

---

## Epic 4 (Optional): Advanced Features & Cloud Deployment

**Goal:** Provide optional Kubernetes deployment configurations for GKE, monitoring capabilities, and advanced features like multiple environment profiles to support teams ready for production deployment.

### Story 4.1: Kubernetes Deployment Configuration for GKE

**As a** DevOps engineer,
**I want** Kubernetes manifests for deploying to GKE,
**so that** I can deploy the application to a production-like cloud environment.

**Acceptance Criteria:**
1. Kubernetes manifests define deployments for frontend and backend services
2. Service definitions expose appropriate ports
3. ConfigMaps and Secrets are used for configuration
4. Cloud SQL proxy configuration for PostgreSQL connection
5. Ingress configuration for routing external traffic
6. Documentation explains GKE setup and deployment process
7. Manifest examples use appropriate resource limits and requests

### Story 4.2: Multiple Environment Profiles

**As a** developer,
**I want** to run different configurations of the stack,
**so that** I can test different scenarios (minimal, full, with/without seeding).

**Acceptance Criteria:**
1. `docker-compose.minimal.yml` runs only backend and database
2. `docker-compose.full.yml` includes all services plus optional monitoring
3. `make dev-minimal` and `make dev-full` commands use appropriate profiles
4. README documents available profiles and when to use each
5. Profile selection can be controlled via environment variable

### Story 4.3: Performance Optimizations

**As a** developer,
**I want** optimized startup performance,
**so that** my environment starts as quickly as possible.

**Acceptance Criteria:**
1. Services start in parallel where dependencies allow
2. Docker images use layer caching effectively
3. Build times are minimized through multi-stage builds
4. Database and Redis use volume mounts for fast restarts
5. Documentation includes tips for improving performance on different OS platforms
6. Benchmark startup time is documented in README

### Story 4.4: CI/CD Pipeline Integration

**As a** developer,
**I want** the environment to work in CI/CD pipelines,
**so that** automated testing can run in consistent environments.

**Acceptance Criteria:**
1. GitHub Actions workflow runs tests in containerized environment
2. Docker Compose configuration works in CI without modifications
3. Test results are reported clearly in CI output
4. Documentation explains how to run tests locally using the same configuration
5. CI configuration is documented in `/docs/ci-cd.md`

---

## Deployment Plan

### Local Development Deployment

**Target Environment:** Developer workstations (macOS, Linux, Windows with WSL2)

**Prerequisites:**
- Docker Desktop or Docker Engine (version 20.10+)
- Docker Compose (version 2.0+)
- Make utility
- Git

**Deployment Steps:**
1. Clone repository: `git clone <repository-url>`
2. Copy environment template: `cp .env.example .env`
3. Review and adjust `.env` variables as needed
4. Start environment: `make dev`
5. Wait for health verification to complete
6. Access frontend at `http://localhost:3000`
7. Access backend API at `http://localhost:3001`

**Rollback:** Run `make down` to stop all services, then `make clean` to remove all data

**Success Criteria:**
- All services start without errors
- Health checks pass for all services
- Frontend accessible in browser
- API responds to health check requests
- Total setup time under 10 minutes

### Optional: Google Kubernetes Engine (GKE) Deployment

**Target Environment:** Google Cloud Platform GKE cluster

**Prerequisites:**
- GCP account with billing enabled
- `gcloud` CLI installed and configured
- `kubectl` CLI installed
- GKE cluster created

**Infrastructure Components:**
- GKE cluster (3 nodes minimum recommended)
- Cloud SQL PostgreSQL instance
- Memorystore Redis instance
- Container Registry for Docker images
- Cloud Load Balancer (via Ingress)

**Deployment Steps:**
1. Build and push Docker images to Container Registry:
   ```bash
   make docker-build
   make docker-push
   ```

2. Create Cloud SQL instance and database:
   ```bash
   gcloud sql instances create <instance-name> --database-version=POSTGRES_15
   ```

3. Create Memorystore Redis instance:
   ```bash
   gcloud redis instances create <instance-name>
   ```

4. Configure Kubernetes secrets:
   ```bash
   kubectl create secret generic app-secrets --from-env-file=.env.production
   ```

5. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f infrastructure/k8s/
   ```

6. Wait for pods to become ready:
   ```bash
   kubectl rollout status deployment/frontend
   kubectl rollout status deployment/backend
   ```

7. Get Ingress IP address:
   ```bash
   kubectl get ingress
   ```

**Configuration Management:**
- Use Kubernetes ConfigMaps for non-sensitive configuration
- Use Kubernetes Secrets for sensitive data (database credentials, API keys)
- Use Cloud Secret Manager for production secrets with GKE integration

**Rollback:**
```bash
kubectl rollout undo deployment/frontend
kubectl rollout undo deployment/backend
```

**Monitoring and Health:**
- Use GKE monitoring dashboards
- Configure Liveness and Readiness probes in pod specs
- Set up Cloud Logging for centralized log aggregation
- Optional: Deploy Prometheus and Grafana for detailed metrics

**Success Criteria:**
- All pods running and healthy
- Services accessible via Ingress IP/domain
- Database connections successful
- Cache connections successful
- Logs flowing to Cloud Logging

**Cost Considerations:**
- GKE cluster costs (compute resources)
- Cloud SQL instance costs
- Memorystore instance costs
- Network egress costs
- Container Registry storage costs

**Documentation Location:**
- Detailed GKE deployment guide: `/docs/deployment/gke-deployment.md`
- Kubernetes manifest templates: `/infrastructure/k8s/`
- CI/CD integration: `/docs/ci-cd.md`

### Deployment Comparison

| Aspect | Local (Docker Compose) | Cloud (GKE) |
|--------|------------------------|-------------|
| **Purpose** | Development, Testing | Staging, Production |
| **Setup Time** | < 10 minutes | 1-2 hours (first time) |
| **Cost** | Free (local resources) | Pay-per-use (GCP billing) |
| **Scalability** | Single machine limits | Horizontal scaling |
| **Persistence** | Local volumes | Cloud SQL, persistent disks |
| **Accessibility** | localhost only | Public IP/domain |
| **Maintenance** | Minimal | Requires cloud operations |

---

## Checklist Results Report

_(This section will be populated after running the PM checklist to validate the PRD completeness and quality)_

---

## Next Steps

### UX Expert Prompt

Review the "User Interface Design Goals" section of this PRD and create detailed design documentation including:
- Wireframes for core screens (welcome page, list view, create/edit forms)
- Component library requirements
- Interaction patterns and user flows
- Responsive design breakpoints
- Error state designs

Use the PRD sections: "User Interface Design Goals" and Epic stories as input for design requirements.

### Architect Prompt

Review this PRD and create a comprehensive architecture document covering:
- Detailed system architecture diagrams
- Service communication patterns
- Database schema design
- API specifications
- Technology stack justification
- Security architecture
- Scalability considerations

Use the PRD sections: "Technical Assumptions", "Requirements", and all Epic stories as input for architecture decisions. Pay special attention to the deployment plan for both local and cloud environments.
