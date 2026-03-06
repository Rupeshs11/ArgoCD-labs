# ArgoCD Image Updater — Automated Image Updates

This folder contains template manifests for configuring **ArgoCD Image Updater** — automatically updates container images in your deployments when new versions are pushed to a container registry.

---

## 📂 Folder Contents

| File                                                     | Description                                                                          |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| [`app.yml`](./app.yml)                                   | ArgoCD Application template with image-updater annotations (semver + Git write-back) |
| [`imageUpdater-secrets.yml`](./imageUpdater-secrets.yml) | Application template with notification annotations (for notification-enabled apps)   |

---

## 🧠 How It Works

```
Container Registry (DockerHub/ECR/GHCR)
        │ new image tag pushed
        ▼
┌──────────────────────────┐
│  ArgoCD Image Updater    │ ← Watches registry for new tags
│  (runs in argocd ns)     │
└──────────┬───────────────┘
           │ detects new version (semver)
           ▼
┌──────────────────────────┐
│  Git Write-Back          │ ← Updates image tag in Git repo
│  (Kustomize/values.yaml) │
└──────────┬───────────────┘
           │
           ▼
┌──────────────────────────┐
│  ArgoCD Auto-Sync        │ ← Detects Git change and deploys
└──────────────────────────┘
```

---

## 📄 Key Annotations in `app.yml`

| Annotation                                                 | Purpose                                        |
| ---------------------------------------------------------- | ---------------------------------------------- |
| `argocd-image-updater.argoproj.io/image-list`              | Maps an alias to a container image             |
| `argocd-image-updater.argoproj.io/write-back-method`       | How to persist updates (Git write-back)        |
| `argocd-image-updater.argoproj.io/<alias>.update-strategy` | Update strategy (`semver`, `latest`, `digest`) |

---

## 🛠️ Prerequisites

1. Install ArgoCD Image Updater:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/argocd-image-updater/stable/manifests/install.yaml
```

2. Create Git credentials secret for write-back:

```bash
kubectl create secret generic argocd-image-updater-git-creds \
  -n argocd \
  --from-literal=username=<your-github-username> \
  --from-literal=password=<your-github-pat>
```

---

## 📍 Configuration Notes

| Config                      | What to Replace                                 |
| --------------------------- | ----------------------------------------------- |
| `<your-dockerhub-username>` | Your Docker Hub username                        |
| `<your-github-username>`    | Your GitHub username                            |
| `image_updater/<app-name>`  | Path in Git repo where Kustomize/manifests live |

---

## ➡️ Related

- Monitoring: [`Monitoring/`](../Monitoring/)
- Notifications: [`argocd-notifications/`](../argocd-notifications/)
