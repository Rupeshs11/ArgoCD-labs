# Security & Scaling — RBAC, User Management, SSO

This folder contains template manifests for configuring **ArgoCD security** — user accounts, RBAC policies, and **GitHub OAuth SSO** integration.

---

## 📂 Folder Contents

| File                                                       | Description                                                  |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| [`user-configmap.yml`](./user-configmap.yml)               | ArgoCD user accounts ConfigMap (template)                    |
| [`rbac-configmap.yml`](./rbac-configmap.yml)               | RBAC policy ConfigMap — role-based access control (template) |
| [`argo-github-configmap.yml`](./argo-github-configmap.yml) | GitHub OAuth SSO ConfigMap (template)                        |
| [`argo-github-rbac.yml`](./argo-github-rbac.yml)           | GitHub SSO RBAC policy mapping (template)                    |
| [`argo-github-secret.yml`](./argo-github-secret.yml)       | GitHub OAuth app credentials Secret (template)               |

> **Note:** These are template files — fill them with your specific configuration before applying.

---

## 🔒 Features

### 1. Local User Management

| File                 | Purpose                                    |
| -------------------- | ------------------------------------------ |
| `user-configmap.yml` | Define local ArgoCD users (beyond `admin`) |
| `rbac-configmap.yml` | Assign roles and permissions to users      |

### 2. GitHub OAuth SSO

| File                        | Purpose                                          |
| --------------------------- | ------------------------------------------------ |
| `argo-github-configmap.yml` | Configure GitHub as OAuth provider               |
| `argo-github-secret.yml`    | Store GitHub OAuth `clientID` and `clientSecret` |
| `argo-github-rbac.yml`      | Map GitHub org/team membership to ArgoCD roles   |

---

## 🛠️ Setup — Local Users & RBAC

```bash
# Apply user config
kubectl apply -f user-configmap.yml

# Apply RBAC policies
kubectl apply -f rbac-configmap.yml

# Restart ArgoCD server to pick up changes
kubectl rollout restart deployment argocd-server -n argocd
```

---

## 🛠️ Setup — GitHub OAuth SSO

### 1. Create GitHub OAuth App

1. Go to **GitHub → Settings → Developer settings → OAuth Apps → New OAuth App**
2. Set:
   - **Homepage URL:** `https://argocd.yourdomain.com`
   - **Callback URL:** `https://argocd.yourdomain.com/api/dex/callback`
3. Note the `clientID` and `clientSecret`

### 2. Apply Manifests

```bash
kubectl apply -f argo-github-secret.yml
kubectl apply -f argo-github-configmap.yml
kubectl apply -f argo-github-rbac.yml

# Restart ArgoCD
kubectl rollout restart deployment argocd-server -n argocd
```

---

## ➡️ Related

- ArgoCD Projects & RBAC: [`gitops-fundamentals/argoCD_features/projects/`](../../gitops-fundamentals/argoCD_features/projects/)
- Notifications: [`argocd-notifications/`](../argocd-notifications/)
