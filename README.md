# ArgoCD Labs — Complete GitOps & ArgoCD Reference

A comprehensive, hands-on repository covering **GitOps fundamentals**, **ArgoCD features**, **advanced operations**, and the **Argo ecosystem** — organized as reusable templates for future reference.

> **GitHub:** [github.com/believer-11/argocd-demos](https://github.com/believer-11/argocd-demos)

---

## 📂 Repository Structure

```
ArgoCD-labs/
│
├── gitops-fundamentals/              # Core GitOps & ArgoCD basics
│   ├── prerequisites/                # Theory (GitOps, ArgoCD) + Setup guide & script
│   ├── argocd-approaches/            # Three ways to deploy: CLI, UI, Declarative
│   └── argoCD_features/              # App of Apps, ApplicationSets, Multi-Cluster, Projects
│
├── gitops-advanced/                  # Production-grade ArgoCD operations
│   ├── Monitoring/                   # Prometheus ServiceMonitors + ArgoCD app template
│   ├── argocd-notifications/         # Email notifications (SMTP config, triggers, templates)
│   ├── argocd_imageUpdater/          # Automated image updates with Git write-back
│   └── security_scaling/             # RBAC, SSO (GitHub OAuth), user management
│
├── argo-ecosystem/                   # Argo Project ecosystem tools
│   ├── argo_workflow/                # Argo Workflows — theory, manifests, installation
│   ├── argo_events/                  # Argo Events — EventSource, Sensors, webhooks
│   └── argo_rollouts/                # Argo Rollouts — Canary & Blue-Green deployments
│
└── argocd_ssl/                       # SSL/TLS setup for ArgoCD with Ingress + cert-manager
    ├── argocd-ingress.yml            # Nginx Ingress for ArgoCD server
    └── letsencrypt-issuer.yml        # Let's Encrypt ClusterIssuer (prod + staging)
```

---

## 🚀 Learning Path

| #   | Module                  | Folder                                           | Topics                                                                       |
| --- | ----------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------- |
| 1   | **GitOps Fundamentals** | [`gitops-fundamentals/`](./gitops-fundamentals/) | GitOps theory, ArgoCD basics, setup, CLI/UI/Declarative approaches, features |
| 2   | **GitOps Advanced**     | [`gitops-advanced/`](./gitops-advanced/)         | Monitoring, notifications, image updater, RBAC, SSO                          |
| 3   | **Argo Ecosystem**      | [`argo-ecosystem/`](./argo-ecosystem/)           | Argo Workflows, Argo Events, Argo Rollouts                                   |
| 4   | **ArgoCD SSL**          | [`argocd_ssl/`](./argocd_ssl/)                   | Production TLS with Ingress + Let's Encrypt                                  |

---

## 🛠️ Prerequisites

| Tool                  | Purpose                  |
| --------------------- | ------------------------ |
| Docker                | Container runtime        |
| Kind / Minikube / EKS | Kubernetes cluster       |
| kubectl               | Kubernetes CLI           |
| Helm                  | Package manager          |
| ArgoCD CLI            | ArgoCD command-line tool |

> Complete setup guide: [`gitops-fundamentals/prerequisites/`](./gitops-fundamentals/prerequisites/)

---

## ⚡ Quick Start

```bash
# Clone the repo
git clone https://github.com/believer-11/argocd-demos.git
cd argocd-demos

# Run the automated setup (Kind cluster + ArgoCD)
chmod +x gitops-fundamentals/prerequisites/03_setup.sh
./gitops-fundamentals/prerequisites/03_setup.sh
```

---

## 📌 Key Links

| Resource            | Link                                                                          |
| ------------------- | ----------------------------------------------------------------------------- |
| ArgoCD Docs         | [argo-cd.readthedocs.io](https://argo-cd.readthedocs.io/)                     |
| Argo Workflows Docs | [argo-workflows.readthedocs.io](https://argo-workflows.readthedocs.io/)       |
| Argo Events Docs    | [argoproj.github.io/argo-events](https://argoproj.github.io/argo-events/)     |
| Argo Rollouts Docs  | [argoproj.github.io/argo-rollouts](https://argoproj.github.io/argo-rollouts/) |

---
