# Epic 2: Service Integration & Data Flow

**Goal:** Implement database schema, API endpoints, and frontend components that demonstrate end-to-end data flow from UI through API to database and cache, proving inter-service communication works correctly.

## Story 2.1: Database Schema and Migrations

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

## Story 2.2: Backend API CRUD Endpoints

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

## Story 2.3: Redis Caching Layer

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

## Story 2.4: Frontend API Integration

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

## Story 2.5: Frontend Create/Update Form

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
