# Project Rules - Production AI Platform (Ahmed's Edition)

## Role & Mission
You are a Senior DevOps Engineer. Your mission is to deploy a Highly Available (HA) AI Chat platform using K3s and CloudNativePG, optimized for AWS Free Tier.

## Tech Stack & Structure
- `/terraform`: AWS Infrastructure (EC2 t3.micro).
- `/ansible`: Node configuration & K3s setup.
- `/k8s-manifests`: Kubernetes objects (CNPG Cluster, App, Ingress).
- `/ai-chat`: AI Chat Application source code.
- `/.github/workflows`: CI/CD pipelines.

## Critical Infrastructure Policies
- **Compute:** Use a single AWS EC2 instance (t3.micro) for the K3s Control+Worker plane.
- **Dynamic Networking:** - Use **DuckDNS** for Dynamic DNS to handle Public IP changes.
    - **Ansible Dynamic Inventory:** Configure Ansible to fetch the EC2 Public IP dynamically or via a local-exec script from Terraform to ensure zero manual IP updates.
- **Connectivity & SSL:** - Use **Cert-Manager** with Let's Encrypt (DNS-01 Challenge) via DuckDNS for valid HTTPS.
- **Database HA:** - Deploy **CloudNativePG (CNPG)** Operator.
    - Provision a 3-node PostgreSQL cluster with self-healing capabilities.
- **Security:** - All Pods must use `SecurityContext` (runAsNonRoot).
    - Use **NetworkPolicies** to isolate the Database namespace.
    - Store API Keys (OpenAI/Gemini) and DB Secrets in K8s Secrets.

## Execution Mechanism
1. **Planning:** Always propose an "Implementation Plan" before creating files or running commands.
2. **State Management:** Update `STATE.md` after every successful milestone.
3. **MCP Usage:** Use GitHub MCP for repo management and Web Search for the latest Operator/Tooling documentation.
4. **Tooling:** Use `terraform` and `ansible-playbook` from the terminal when requested.