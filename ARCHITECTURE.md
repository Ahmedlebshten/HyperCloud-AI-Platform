# System Architecture - Production AI Platform

## 1. Infrastructure Layer (AWS)
- **Compute:** 1x EC2 Instance (t3.micro).
- **Networking:** Public Subnet with Elastic IP (or dynamic via DuckDNS).
- **Security:** Security Group opening ports 22 (SSH), 80 (HTTP), 443 (HTTPS), and 6443 (K3s API).

## 2. Platform Layer (K3s)
- **Distro:** K3s (Lightweight Kubernetes).
- **Storage:** Local-path provisioner (default in K3s).
- **Ingress:** Traefik (Built-in) managed via HelmChartConfig.
- **Certificates:** Cert-Manager with Let's Encrypt (DNS-01 Challenge via DuckDNS).

## 3. Data & Application Layer
- **Database:** PostgreSQL HA Cluster (3 nodes) managed by CloudNativePG Operator.
- **Application:** Chatbot-UI (Next.js) running in a 2-replica deployment.
- **Secrets:** Managed via K8s Secrets for API Keys and DB credentials.

## 4. CI/CD Flow
- **CI:** GitHub Actions builds Docker Image -> Pushes to DockerHub.
- **CD:** GitHub Actions triggers `kubectl rollout restart` or updates image tag.