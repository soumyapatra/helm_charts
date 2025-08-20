# Superbill API – Kubernetes Manifests

This repo contains Kubernetes manifests for deploying the **Superbill API** on AKS.

## 📦 Components

- `deployment.yaml` → Application Deployment (pods, image, env vars)
- `service.yaml` → Internal Service (ClusterIP on port 80 → container 8080)
- `ingress.yaml` → Ingress rules + TLS for external access
- `secrets/` → Placeholder secrets (`superbill-conn`, `superbill-tls`)

## 🚀 Provisioning Steps

1. **Create namespace (if not exists):**
```bash
   kubectl create ns superbill
```

2. **Provision Secrets:**

For DB connection:
```bash
kubectl apply -f secrets/superbill-conn-secret.yaml
```

For TLS (if not using cert-manager):
```bash
kubectl apply -f secrets/superbill-tls-secret.yaml
```

3. **Deploy Application:**
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

4. **Check rollout:**

kubectl rollout status deploy/superbill-api -n superbill


5. **Verify access:**

```bash
curl -vk https://superbillapid.zayniq.org/
curl -vk https://superbilluid.zayniq.org/
```


Notes

   Deployment uses RollingUpdate strategy with maxSurge: 25% and maxUnavailable: 25%.

   DB connection string is injected from superbill-conn secret.

   TLS termination is handled by NGINX Ingress with secret superbill-tls.