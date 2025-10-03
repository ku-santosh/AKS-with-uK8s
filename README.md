# React + TypeScript + Vite UI on AKS via GitLab CI/CD

Production-ready skeleton for a **React + TypeScript + Vite** frontend deployed to **Azure Kubernetes Service (AKS)** with **GitLab CI/CD** across **Dev / UAT / Prod** namespaces.

## Features
- Dev/UAT/Prod deployments via GitLab CI/CD (Dev & UAT on MR merge, Prod requires manual approval)
- Environment configs via `.env.development`, `.env.uat`, `.env.production` (Vite requires `VITE_` prefix)
- Kustomize overlays per environment (namespace, host, image tag)
- Dockerized static site served by Nginx
- Optional scoped RBAC for GitLab deployer

## Quick Start (Local)
```bash
npm install
npm run dev        # local dev server
npm run build      # production build to /dist
npm run preview    # preview /dist locally
```

## Environment Variables (Vite)
Create these at repo root:
```
.env.development
.env.uat
.env.production
```
Each file should use `VITE_` prefix, e.g.:
```env
VITE_API_URL=https://uat-api.mycompany.com
VITE_ENV=uat
```

## Build & Container
The Dockerfile copies `.env.$ENV` → `.env` and runs `vite build`, then serves `/dist` via Nginx.
```bash
docker build --build-arg ENV=development -t react-ui:dev .
docker run -p 8080:80 react-ui:dev
```

## Kubernetes
Create namespaces:
```bash
kubectl apply -f k8s/namespaces/namespace-dev.yaml
kubectl apply -f k8s/namespaces/namespace-uat.yaml
kubectl apply -f k8s/namespaces/namespace-prod.yaml
```
Deploy with Kustomize:
```bash
kubectl apply -k k8s/dev
kubectl apply -k k8s/uat
kubectl apply -k k8s/prod
```

## GitLab CI/CD
- Set CI variables: `AZURE_ACR_USERNAME`, `AZURE_ACR_PASSWORD`, `KUBE_SERVER`, `KUBE_CA`, `KUBE_TOKEN`
- Branch mapping: `dev`→Dev, `uat`→UAT, `main`→Prod (manual gate)
- `.gitlab-ci.yml` builds image with `--build-arg ENV=...` and deploys with `kubectl apply -k`

## Optional RBAC
Use `k8s/rbac/gitlab-deployer-dev.yaml` (and equivalents for UAT/Prod) to avoid cluster-admin. Then set environment-specific tokens in GitLab and adjust deploy jobs to use those.

## Next Steps / Hardening
- Add TLS via cert-manager and Ingress `tls:` block
- Use commit SHA image tags alongside `*-latest` for rollbacks
- Add lint/tests & container security scanning in CI
- Consider ConfigMaps for runtime configuration (to avoid rebuilds for API URL changes)

---

## 🔸 Optional: GitLab Secrets & RBAC

- For **MVP**, the pipeline uses a single `KUBE_TOKEN` (cluster-admin) for simplicity.
- Later, you can create scoped ServiceAccounts (see `k8s/rbac/`) and set separate GitLab secrets:
  - `KUBE_TOKEN_DEV`
  - `KUBE_TOKEN_UAT`
  - `KUBE_TOKEN_PROD`
- Replace the token references in `.gitlab-ci.yml` with these values.

This hardening step is recommended for **Phase 2 (Production Readiness)**.
