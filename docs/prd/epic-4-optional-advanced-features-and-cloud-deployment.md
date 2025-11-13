# Epic 4 (Optional): Advanced Features & Cloud Deployment

**Goal:** Provide optional Kubernetes deployment configurations for GKE, monitoring capabilities, and advanced features like multiple environment profiles to support teams ready for production deployment.

## Story 4.1: Kubernetes Deployment Configuration for GKE

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

## Story 4.2: Multiple Environment Profiles

**As a** developer,
**I want** to run different configurations of the stack,
**so that** I can test different scenarios (minimal, full, with/without seeding).

**Acceptance Criteria:**
1. `docker-compose.minimal.yml` runs only backend and database
2. `docker-compose.full.yml` includes all services plus optional monitoring
3. `make dev-minimal` and `make dev-full` commands use appropriate profiles
4. README documents available profiles and when to use each
5. Profile selection can be controlled via environment variable

## Story 4.3: Performance Optimizations

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

## Story 4.4: CI/CD Pipeline Integration

**As a** developer,
**I want** the environment to work in CI/CD pipelines,
**so that** automated testing can run in consistent environments.

**Acceptance Criteria:**
1. GitHub Actions workflow runs tests in containerized environment
2. Docker Compose configuration works in CI without modifications
3. Test results are reported clearly in CI output
4. Documentation explains how to run tests locally using the same configuration
5. CI configuration is documented in `/docs/ci-cd.md`
