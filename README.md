# Next.js • Docker • GHCR • Minikube

A minimal Next.js app containerized with an optimized multi-stage Dockerfile, built & pushed to **GitHub Container Registry (GHCR)** via GitHub Actions, and deployable to Kubernetes/Minikube.

## Prerequisites
- Node 20.x, npm
- Docker
- kubectl + Minikube
- GitHub account

## Local Development
\`\`\`bash
npm ci
npm run dev
# http://localhost:3000
curl http://localhost:3000/api/health
\`\`\`

## Docker (local)

\`\`\`bash
docker build -t nextjs-local:dev .
docker run --rm -p 3000:3000 nextjs-local:dev
curl http://localhost:3000/api/health
\`\`\`

## GitHub Actions → GHCR

Push to `main`; workflow builds and publishes to:

\`\`\`
ghcr.io/<YOUR_GITHUB_USERNAME>/nextjs-docker-ghcr-minikube
\`\`\`

Tags: `latest` (default branch), `sha-<commit>`, and git tag.

## Deploy to Minikube

\`\`\`bash
minikube start
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl rollout status deployment/nextjs-app
minikube service nextjs-service --url
\`\`\`

Open `http://<MINIKUBE_IP>:30080` and check `/api/health`.

## Troubleshooting

**Image pull (private):**

\`\`\`bash
kubectl create secret docker-registry ghcr-pull \
  --docker-server=ghcr.io \
  --docker-username=<YOUR_GITHUB_USERNAME> \
  --docker-password=<TOKEN> \
  --docker-email=<YOUR_EMAIL>
\`\`\`

Add under `spec.template.spec`:

\`\`\`yaml
imagePullSecrets:
  - name: ghcr-pull
\`\`\`

## Submission Email

**Subject:** `DevOps Assessment Submission - <YOUR_NAME>`

* Repository URL: `https://github.com/<YOUR_GITHUB_USERNAME>/nextjs-docker-ghcr-minikube`
* GHCR Image URL: `https://ghcr.io/<YOUR_GITHUB_USERNAME>/nextjs-docker-ghcr-minikube`
