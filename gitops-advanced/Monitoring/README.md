# Monitoring ArgoCD with Prometheus

This folder contains template manifests for monitoring **ArgoCD components** using **Prometheus Operator** (ServiceMonitors) and a sample ArgoCD Application with **Image Updater** annotations.

---

## 📂 Folder Contents

| File                                                   | Description                                                                                  |
| ------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| [`app.yml`](./app.yml)                                 | ArgoCD Application template with image-updater annotations (semver strategy, Git write-back) |
| [`argo_serviceMonitor.yml`](./argo_serviceMonitor.yml) | ServiceMonitor resources for ArgoCD metrics (metrics server, API server, repo server)        |

---

## 📄 ServiceMonitors (`argo_serviceMonitor.yml`)

Defines **three ServiceMonitors** for Prometheus to scrape ArgoCD metrics:

| ServiceMonitor               | Targets                               | Label Selector                                  |
| ---------------------------- | ------------------------------------- | ----------------------------------------------- |
| `argocd-metrics`             | ArgoCD application controller metrics | `app.kubernetes.io/name: argocd-metrics`        |
| `argocd-server-metrics`      | ArgoCD API server metrics             | `app.kubernetes.io/name: argocd-server-metrics` |
| `argocd-repo-server-metrics` | ArgoCD repo server metrics            | `app.kubernetes.io/name: argocd-repo-server`    |

---

## 🛠️ Prerequisites

1. **Prometheus Operator** / **kube-prometheus-stack** installed in your cluster
2. **ArgoCD** installed in the `argocd` namespace

---

## 🚀 Usage

```bash
# Apply ServiceMonitors
kubectl apply -f argo_serviceMonitor.yml

# Verify they are picked up by Prometheus
kubectl get servicemonitors -n argocd

# Apply sample app (customize first)
kubectl apply -f app.yml
```

---

## 📍 Configuration Notes

- The `release: kube-prometheus-stack` label must match your Prometheus Operator's release label
- Verify ArgoCD service labels with: `kubectl get svc -n argocd --show-labels`
- The `app.yml` template uses `<your-dockerhub-username>` and `<your-github-username>` — **replace** with your values

---

## ➡️ Related

- Notifications: [`argocd-notifications/`](../argocd-notifications/)
- Image Updater: [`argocd_imageUpdater/`](../argocd_imageUpdater/)
