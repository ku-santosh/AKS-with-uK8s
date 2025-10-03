# 📌 Useful kubectl Commands for AKS (uk8s)

This cheat sheet provides the most common `kubectl` commands with **comments** for working with **Azure Kubernetes Service (AKS / uk8s)** in Dev, UAT, and Prod environments.

---

## 🔹 Context & Cluster Info
```bash
kubectl config get-contexts            # List all contexts
kubectl config use-context <context>   # Switch to AKS context
kubectl cluster-info                   # Show AKS cluster info (API server, DNS, etc.)
kubectl get nodes -o wide              # List AKS nodes with IPs
kubectl describe node <node-name>      # Detailed node info
```

---

## 🔹 Namespaces
```bash
kubectl get ns                         # List all namespaces
kubectl create ns at41457-dev-recsdiui-dev     # Create Dev namespace
kubectl delete ns at41457-uat-recsdiui-uat     # Delete UAT namespace
kubectl config set-context --current --namespace=at41457-dev-recsdiui-dev   # Set default namespace
```

---

## 🔹 Deployments & Pods
```bash
kubectl get deployments -n at41457-dev-recsdiui-dev        # List deployments in Dev
kubectl describe deployment react-ui -n at41457-uat-recsdiui-uat   # Debug deployment in UAT
kubectl rollout status deployment react-ui -n at41457-prod-recsdiui-prod   # Check rollout progress in Prod
kubectl rollout undo deployment react-ui -n at41457-prod-recsdiui-prod     # Rollback last Prod deployment

kubectl get pods -n at41457-dev-recsdiui-dev               # List pods in Dev
kubectl describe pod <pod-name> -n at41457-dev-recsdiui-dev  # Debug pod
kubectl logs <pod-name> -n at41457-dev-recsdiui-dev          # View pod logs
kubectl logs -f <pod-name> -n at41457-dev-recsdiui-dev       # Stream logs
kubectl exec -it <pod-name> -n at41457-uat-recsdiui-uat -- sh   # Exec into pod
```

---

## 🔹 Services & Ingress
```bash
kubectl get svc -n at41457-dev-recsdiui-dev       # List services in Dev
kubectl describe svc react-ui-service -n at41457-uat-recsdiui-uat   # Debug service in UAT

kubectl get ingress -n at41457-prod-recsdiui-prod # List ingress rules in Prod
kubectl describe ingress react-ui-ingress -n at41457-prod-recsdiui-prod  # Debug ingress
```

---

## 🔹 ConfigMaps & Secrets
```bash
kubectl get configmaps -n at41457-dev-recsdiui-dev           # List configmaps in Dev
kubectl describe configmap <name> -n at41457-uat-recsdiui-uat # Debug a configmap

kubectl get secrets -n at41457-prod-recsdiui-prod             # List secrets in Prod
kubectl describe secret <name> -n at41457-prod-recsdiui-prod  # Debug a secret
kubectl get secret <name> -n at41457-prod-recsdiui-prod -o jsonpath="{.data.key}" | base64 --decode   # Decode a secret
```

---

## 🔹 Events & Debugging
```bash
kubectl get events -n at41457-dev-recsdiui-dev --sort-by=.metadata.creationTimestamp   # Recent events in Dev
kubectl describe pod <pod-name> -n at41457-uat-recsdiui-uat                            # Pod details
kubectl top pods -n at41457-prod-recsdiui-prod                                        # Pod CPU/Memory usage
kubectl top nodes                                                                     # Node CPU/Memory usage
```

---

## 🔹 Apply & Delete Manifests
```bash
kubectl apply -f k8s/namespaces/namespace-dev.yaml   # Apply single manifest
kubectl apply -k k8s/dev                             # Apply Kustomize overlay (Dev)

kubectl delete -f k8s/namespaces/namespace-uat.yaml  # Delete UAT namespace manifest
kubectl delete -k k8s/prod                           # Delete all Prod resources
```

---

## 🔹 Scaling
```bash
kubectl scale deployment react-ui --replicas=3 -n at41457-uat-recsdiui-uat   # Scale UAT deployment to 3 pods
kubectl autoscale deployment react-ui -n at41457-prod-recsdiui-prod --min=2 --max=5 --cpu-percent=70   # HPA
```

---

## 🔹 Access Application (Port-Forward)
```bash
kubectl port-forward svc/react-ui-service 3000:80 -n at41457-dev-recsdiui-dev
# Opens http://localhost:3000 → maps to Dev service port 80
```

---

✅ These commands cover **90% of daily AKS operations**:  
- Managing **namespaces, pods, deployments**  
- Debugging issues (logs, events, describe)  
- Checking **Ingress/DNS**  
- Scaling, rolling back, port-forward for local testing  
