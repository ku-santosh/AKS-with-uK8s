# 🚑 Troubleshooting Guide

This document will track **common issues** and their **solutions** when working with the React UI app on AKS.

---

## 🔹 Common Issues

- **Pod stuck in ImagePullBackOff**
  - Check if Docker image exists in ACR and GitLab CI/CD variables for ACR login are correct.

- **Ingress not resolving**
  - Ensure DNS records are configured and pointing to AKS ingress controller IP.

- **Environment variables missing**
  - Vite requires `VITE_` prefix for all env vars. Double-check `.env.*` files.

---

## 🔹 TODO
- Expand with real-world issues as they occur.
