# Prerequisites — Theory & Setup

This folder contains the foundational theory behind **GitOps** and **ArgoCD**, along with a complete setup guide to install ArgoCD on a Kind cluster.

---

## 📂 Folder Contents

| File                                           | Description                                                                                     |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| [`01_Introduction.md`](./01_Introduction.md)   | What is GitOps, core principles, GitOps vs Traditional CI/CD                                    |
| [`02_argocd_basics.md`](./02_argocd_basics.md) | ArgoCD architecture, components, key concepts (Applications, Projects, Sync, Health, Rollbacks) |
| [`03_setup.md`](./03_setup.md)                 | Step-by-step guide to install ArgoCD on a Kind cluster (Helm & Manifest methods)                |
| [`03_setup.sh`](./03_setup.sh)                 | Automated bash script that creates a Kind cluster and installs ArgoCD (UI + CLI)                |

---

## 📖 Reading Order

1. **Start with** → [`01_Introduction.md`](./01_Introduction.md) — Understand GitOps fundamentals
2. **Then read** → [`02_argocd_basics.md`](./02_argocd_basics.md) — Learn ArgoCD architecture & concepts
3. **Finally** → [`03_setup.md`](./03_setup.md) — Set up your own ArgoCD environment

---

## ⚡ Quick Setup

You can either follow `03_setup.md` step-by-step, or run the automated script:

```bash
chmod +x 03_setup.sh
./03_setup.sh
```

> **Note:** Before running, update the `apiServerAddress` in the script/config with your EC2 private IP.

---

## 🔗 Required Tools

| Tool       | Purpose                                   | Install Guide                                                                    |
| ---------- | ----------------------------------------- | -------------------------------------------------------------------------------- |
| Docker     | Container runtime for Kind                | `apt install docker.io`                                                          |
| Kind       | Kubernetes in Docker                      | [kind.sigs.k8s.io](https://kind.sigs.k8s.io/docs/user/quick-start/#installation) |
| kubectl    | Kubernetes CLI                            | [kubernetes.io](https://kubernetes.io/docs/tasks/tools/install-kubectl/)         |
| Helm       | Package manager (for Helm install method) | [helm.sh](https://helm.sh/docs/intro/install/)                                   |
| ArgoCD CLI | ArgoCD command-line tool                  | Installed via `03_setup.sh`                                                      |

---

## ➡️ Next Steps

Once ArgoCD is installed and running, proceed to [`argocd-approaches/`](../argocd-approaches/) to deploy your first application.
