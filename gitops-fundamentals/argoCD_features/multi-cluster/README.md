# Multi-Cluster Deployments

This folder demonstrates how to deploy the same application across **multiple Kubernetes clusters** (or environments) using ArgoCD. Each YAML file defines an Application targeting a different cluster/environment.

---

## 📂 Folder Contents

| File                             | Description                                                |
| -------------------------------- | ---------------------------------------------------------- |
| [`dev.yml`](./dev.yml)           | Application deployed to the **Dev** cluster (Nginx)        |
| [`stagging.yml`](./stagging.yml) | Application deployed to the **Staging** cluster (Apache)   |
| [`prod.yml`](./prod.yml)         | Application deployed to the **Production** cluster (Nginx) |

---

## 🧠 How Multi-Cluster Works

```
              ArgoCD Server
              (central control)
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   ┌─────────┐ ┌──────────┐ ┌─────────┐
   │   Dev   │ │ Staging  │ │  Prod   │
   │ Cluster │ │ Cluster  │ │ Cluster │
   └─────────┘ └──────────┘ └─────────┘
```

1. **Register clusters** with ArgoCD using `argocd cluster add <context-name>`
2. Each Application YAML specifies a **different `destination.server`** URL
3. ArgoCD deploys and manages apps across all clusters from a single instance

---

## 🛠️ Usage

### Step 1: Register Your Clusters

```bash
# List available contexts
kubectl config get-contexts

# Add clusters to ArgoCD
argocd cluster add kind-dev-cluster --name dev-cluster --insecure
argocd cluster add kind-staging-cluster --name staging-cluster --insecure
argocd cluster add kind-prod-cluster --name prod-cluster --insecure

# Verify registered clusters
argocd cluster list
```

### Step 2: Update Destination Server URLs

In each YAML file, replace the `destination.server` with the actual cluster URL from `argocd cluster list`:

```yaml
destination:
  server: https://<cluster-ip>:<port> # From argocd cluster list
  namespace: default
```

### Step 3: Apply the Applications

```bash
kubectl apply -f dev.yml
kubectl apply -f stagging.yml
kubectl apply -f prod.yml
```

### Step 4: Verify

```bash
argocd app list
```

---

## 📍 Required Paths & Configuration

| Config               | Purpose                                                           |
| -------------------- | ----------------------------------------------------------------- |
| `destination.server` | Target cluster URL (get from `argocd cluster list`)               |
| `source.path`        | Path in Git repo where manifests live (e.g., `ui_approach/nginx`) |
| `source.repoURL`     | Your Git repository URL                                           |
| `argocd` namespace   | Where Application CRDs are created                                |

---

## ✅ Best Practices

- Use distinct app names per environment (e.g., `nginx-dev`, `nginx-stg`, `nginx-prod`)
- For **large-scale multi-cluster**, use [`ApplicationSets`](../ApplicationSets/) with the **Cluster Generator** instead
- Keep environment-specific configs in separate directories or use **Kustomize overlays**
- Always verify cluster URLs with `argocd cluster list` before applying

---

## ➡️ Next Steps

- Explore [`ApplicationSets/`](../ApplicationSets/) for automated multi-cluster deployment at scale
- Learn [`projects/`](../projects/) to restrict which clusters teams can target
