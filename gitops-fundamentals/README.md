# GitOps Fundamentals with ArgoCD

A comprehensive hands-on guide to learning **GitOps** and **ArgoCD** — from theory to practical deployments on Kubernetes.

---

## 📂 Repository Structure

```
gitops-fundamentals/
├── prerequisites/                    # Theory + Setup guides
│   ├── 01_Introduction.md           # GitOps concepts & principles
│   ├── 02_argocd_basics.md          # ArgoCD architecture & key concepts
│   ├── 03_setup.md                  # Step-by-step ArgoCD installation guide
│   └── 03_setup.sh                  # Automated setup script (Kind + ArgoCD)
│
├── argocd-approaches/               # Three ways to deploy apps with ArgoCD
│   ├── cli_approach.md              # Deploy apps using ArgoCD CLI
│   ├── UI_approach.md               # Deploy apps using ArgoCD Web UI
│   ├── declarative_approach.md      # Deploy apps using YAML manifests (GitOps way)
│   └── outputs/                     # Output screenshots (if any)
│
└── argoCD_features/                 # Advanced ArgoCD features
    ├── app_of_app/                  # App of Apps pattern
    │   └── root.yml                 # Root Application manifest
    ├── ApplicationSets/             # ApplicationSet generators
    │   ├── list_generator.yml       # List generator example
    │   ├── git_generator.yml        # Git directory generator example
    │   └── cluster_generator.yml    # Cluster generator example
    ├── multi-cluster/               # Multi-cluster deployment
    │   ├── dev.yml                  # Dev cluster Application
    │   ├── stagging.yml             # Staging cluster Application
    │   └── prod.yml                 # Production cluster Application
    └── projects/                    # ArgoCD Projects & RBAC
        ├── project.yml              # AppProject definition
        └── nginx.yml                # Application scoped to a Project
```

---

## 🚀 Learning Path

| #   | Topic                | Folder                                                                   | Description                                             |
| --- | -------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------- |
| 1   | GitOps Theory        | [`prerequisites/`](./prerequisites/)                                     | What is GitOps, principles, GitOps vs CI/CD             |
| 2   | ArgoCD Basics        | [`prerequisites/`](./prerequisites/)                                     | Architecture, key concepts, comparison with FluxCD      |
| 3   | Setup & Install      | [`prerequisites/`](./prerequisites/)                                     | Kind cluster + ArgoCD (Helm / Manifests) + CLI setup    |
| 4   | CLI Approach         | [`argocd-approaches/`](./argocd-approaches/)                             | Deploy an app using `argocd` CLI commands               |
| 5   | UI Approach          | [`argocd-approaches/`](./argocd-approaches/)                             | Deploy an app through ArgoCD Web Dashboard              |
| 6   | Declarative Approach | [`argocd-approaches/`](./argocd-approaches/)                             | Deploy using `Application` YAML manifests (true GitOps) |
| 7   | App of Apps          | [`argoCD_features/app_of_app/`](./argoCD_features/app_of_app/)           | Manage multiple apps with a single root Application     |
| 8   | ApplicationSets      | [`argoCD_features/ApplicationSets/`](./argoCD_features/ApplicationSets/) | Auto-generate apps using generators                     |
| 9   | Multi-Cluster        | [`argoCD_features/multi-cluster/`](./argoCD_features/multi-cluster/)     | Deploy same app to multiple clusters/environments       |
| 10  | Projects & RBAC      | [`argoCD_features/projects/`](./argoCD_features/projects/)               | Restrict teams using AppProjects                        |

---

## 🛠️ Prerequisites

- **Docker** — Container runtime
- **Kind** — Kubernetes in Docker ([Install Guide](https://kind.sigs.k8s.io/docs/user/quick-start/#installation))
- **kubectl** — Kubernetes CLI ([Install Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/))
- **Helm** — Package manager ([Install Guide](https://helm.sh/docs/intro/install/))
- **ArgoCD CLI** — ArgoCD command-line tool

> Refer to [`prerequisites/03_setup.md`](./prerequisites/03_setup.md) for complete installation instructions.

---

## ⚡ Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/argocd-demos.git
cd argocd-demos

# 2. Run the automated setup script
chmod +x gitops-fundamentals/prerequisites/03_setup.sh
./gitops-fundamentals/prerequisites/03_setup.sh

# 3. Follow the approaches in order: CLI → UI → Declarative
```

---
