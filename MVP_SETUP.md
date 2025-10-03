# ✅ MVP Setup Checklist

Follow these steps to get the React + TS + Vite app deployed via GitLab CI/CD to AKS.

---

## 1. Prerequisites
- Azure AKS cluster running
- Azure Container Registry (ACR)
- GitLab project created

---

## 2. Configure GitLab Variables
Go to **Settings > CI/CD > Variables** in GitLab and add:

- `AZURE_ACR_USERNAME`
- `AZURE_ACR_PASSWORD`
- `KUBE_SERVER`
- `KUBE_CA`
- `KUBE_TOKEN` (🔸 cluster-admin for MVP; later replace with RBAC tokens)

---

## 3. Kubernetes Setup
Apply namespaces:
```bash
kubectl apply -f k8s/namespaces/
```

Verify:
```bash
kubectl get ns
```

---

## 4. DNS & Ingress
- Update DNS records for:
  - `reactui-dev.mycompany.com`
  - `reactui-uat.mycompany.com`
  - `reactui.mycompany.com`
- Ensure they point to your AKS ingress controller.

---

## 5. Pipeline Flow
- Create feature branch → run builds only.
- Merge to `dev` → auto deploy to Dev namespace.
- Merge to `uat` → auto deploy to UAT namespace.
- Merge to `main` → manual approval required → deploy to Prod namespace.

---

## 6. Branch Protection (GitLab)
- Protect `dev`, `uat`, `main` branches.
- Allow merges only via Merge Requests.
- Require reviews as per environment:
  - Dev → 1 reviewer
  - UAT → 2 reviewers
  - Prod → 2+ reviewers

---

✅ With this, you have a fully working MVP setup. 
Later, see README Phase 2 for hardening (TLS, RBAC, monitoring).
