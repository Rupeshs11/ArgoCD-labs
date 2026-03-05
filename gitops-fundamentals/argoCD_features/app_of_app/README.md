# App of Apps Pattern

The **App of Apps** pattern is a powerful ArgoCD feature where a single **root Application** manages multiple **child Applications**. Instead of manually creating each app, you define one root app that points to a directory containing child Application manifests — ArgoCD will automatically create and manage them all.

---

## 📂 Folder Contents

| File                     | Description                                           |
| ------------------------ | ----------------------------------------------------- |
| [`root.yml`](./root.yml) | Root Application manifest that manages all child apps |

---

## 🧠 How It Works

```
                ┌──────────────┐
                │   Root App   │ ← You create only this
                └──────┬───────┘
                       │
           ┌───────────┼───────────┐
           ▼           ▼           ▼
     ┌──────────┐ ┌──────────┐ ┌──────────┐
     │ Child A  │ │ Child B  │ │ Child C  │  ← ArgoCD creates these
     └──────────┘ └──────────┘ └──────────┘
```

1. **Root App** points to a Git directory (e.g., `app_of_apps/apps/`) that contains child `Application` YAML files
2. ArgoCD syncs the root app and discovers the child Application manifests
3. Each child Application is created and managed automatically
4. If a child manifest is removed from Git, ArgoCD prunes the app (with `prune: true`)

---

## 📄 Root Application Manifest (`root.yml`)

```yaml
source:
  repoURL: https://github.com/beleiver-11/argocd-demos.git
  targetRevision: main
  path: app_of_apps/apps # Directory containing child Application YAMLs
destination:
  server: <argocd_cluster_server_url>
  namespace: argocd # Applications CRDs always live in argocd namespace
syncPolicy:
  automated:
    prune: true # Remove child apps if their manifest is deleted
    selfHeal: true # Auto-fix drift
```

---

## 🛠️ Usage

### Deploy the Root App

```bash
kubectl apply -f root.yml
```

### Verify

```bash
# Check root app
argocd app get root-app

# List all apps (should show root + child apps)
argocd app list
```

---

## ✅ Best Practices

- Store all child Application YAMLs in a dedicated directory (e.g., `app_of_apps/apps/`)
- Enable `prune: true` and `selfHeal: true` on the root app
- Use this pattern when managing **5+ related applications**
- Combine with **Projects** to restrict access per team

---

## 📍 Required Paths

| Path                | Purpose                                                            |
| ------------------- | ------------------------------------------------------------------ |
| `app_of_apps/apps/` | Directory in Git repo where child Application manifests are stored |
| `argocd` namespace  | Where the root and child Application CRDs are created              |

---

## ➡️ Next Steps

- Explore [`ApplicationSets/`](../ApplicationSets/) for even more scalable app generation
- Learn [`projects/`](../projects/) to restrict access for teams
