# GitOps Advanced — Production-Grade ArgoCD Operations

This folder contains templates and configurations for **production-grade ArgoCD operations** — monitoring, notifications, automated image updates, and security/RBAC.

---

## 📂 Folder Structure

```
gitops-advanced/
├── Monitoring/                # Prometheus monitoring for ArgoCD
│   ├── app.yml                # ArgoCD Application template with image updater annotations
│   └── argo_serviceMonitor.yml # ServiceMonitor resources for ArgoCD components
│
├── argocd-notifications/      # Email notifications for ArgoCD events
│   ├── app.yml                # Application with notification annotations
│   ├── notification-configmap.yml # SMTP config, templates, triggers
│   └── smtp-secrets.yml       # SMTP credentials Secret
│
├── argocd_imageUpdater/       # Automated container image updates
│   ├── app.yml                # Application with image-updater annotations
│   └── imageUpdater-secrets.yml # Application with notification annotations (template)
│
└── security_scaling/          # RBAC, user management, SSO
    ├── user-configmap.yml     # ArgoCD user accounts config (template)
    ├── rbac-configmap.yml     # RBAC policy config (template)
    ├── argo-github-configmap.yml  # GitHub OAuth SSO config (template)
    ├── argo-github-rbac.yml   # GitHub SSO RBAC policies (template)
    └── argo-github-secret.yml # GitHub OAuth credentials (template)
```

---

## 🔄 Feature Overview

| Feature             | Folder                                             | Purpose                                       |
| ------------------- | -------------------------------------------------- | --------------------------------------------- |
| **Monitoring**      | [`Monitoring/`](./Monitoring/)                     | Prometheus ServiceMonitors for ArgoCD metrics |
| **Notifications**   | [`argocd-notifications/`](./argocd-notifications/) | Email alerts on deploy/health events          |
| **Image Updater**   | [`argocd_imageUpdater/`](./argocd_imageUpdater/)   | Auto-update container images from registry    |
| **Security & RBAC** | [`security_scaling/`](./security_scaling/)         | User management, RBAC policies, GitHub SSO    |

---

## 📖 Learning Order

1. **Monitoring** → Set up Prometheus metrics for ArgoCD
2. **Notifications** → Configure email alerts for deployments
3. **Image Updater** → Automate image updates with Git write-back
4. **Security & RBAC** → User accounts, roles, and SSO

---

## ➡️ Related

- ArgoCD Fundamentals: [`gitops-fundamentals/`](../gitops-fundamentals/)
- Argo Ecosystem: [`argo-ecosystem/`](../argo-ecosystem/)
- Full project: [github.com/believer-11/argocd-demos](https://github.com/believer-11/argocd-demos)
