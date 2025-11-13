# Deployment Plan

## Local Development Deployment

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

## Optional: Google Kubernetes Engine (GKE) Deployment

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

## Deployment Comparison

| Aspect | Local (Docker Compose) | Cloud (GKE) |
|--------|------------------------|-------------|
| **Purpose** | Development, Testing | Staging, Production |
| **Setup Time** | < 10 minutes | 1-2 hours (first time) |
| **Cost** | Free (local resources) | Pay-per-use (GCP billing) |
| **Scalability** | Single machine limits | Horizontal scaling |
| **Persistence** | Local volumes | Cloud SQL, persistent disks |
| **Accessibility** | localhost only | Public IP/domain |
| **Maintenance** | Minimal | Requires cloud operations |
