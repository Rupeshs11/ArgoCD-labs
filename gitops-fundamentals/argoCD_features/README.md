# ArgoCD Advanced Features

This folder contains practical examples of **advanced ArgoCD features** that are essential for managing real-world Kubernetes deployments. Each sub-folder demonstrates a specific pattern with ready-to-use YAML manifests.

---

## 📂 Folder Structure

```
argoCD_features/
├── app_of_app/               # App of Apps pattern
│   └── root.yml              # Root Application that manages child apps
├── ApplicationSets/          # ApplicationSet generators
│   ├── list_generator.yml    # Static list of apps
│   ├── git_generator.yml     # Auto-discover apps from Git directories
│   └── cluster_generator.yml # Auto-deploy to all registered clusters
├── multi-cluster/            # Multi-cluster/environment deployments
│   ├── dev.yml               # Application for Dev cluster
│   ├── stagging.yml          # Application for Staging cluster
│   └── prod.yml              # Application for Production cluster
└── projects/                 # ArgoCD Projects & RBAC
    ├── project.yml           # AppProject with role-based access
    └── nginx.yml             # Application scoped to a Project
```

---

## 🔄 Feature Overview

| Feature             | Folder                                   | Purpose                                     | When to Use                                  |
| ------------------- | ---------------------------------------- | ------------------------------------------- | -------------------------------------------- |
| **App of Apps**     | [`app_of_app/`](./app_of_app/)           | One root app manages many child apps        | Managing a suite of microservices            |
| **ApplicationSets** | [`ApplicationSets/`](./ApplicationSets/) | Auto-generate Applications using generators | Deploying to multiple clusters/envs at scale |
| **Multi-Cluster**   | [`multi-cluster/`](./multi-cluster/)     | Deploy to dev, staging, prod clusters       | Environment-based deployments                |
| **Projects**        | [`projects/`](./projects/)               | RBAC, repo/cluster restrictions per team    | Multi-team organizations                     |

---

## 📖 Recommended Order

1. **App of Apps** → Understand hierarchical app management
2. **Multi-Cluster** → Learn environment-based deployments
3. **Projects** → Implement team-level access control
4. **ApplicationSets** → Scale deployments with generators

---

## 🛠️ Prerequisites

- ArgoCD installed and running on your cluster
- ArgoCD CLI configured and logged in
- Git repository with your Kubernetes manifests
- For multi-cluster: Multiple clusters registered via `argocd cluster add`

> Setup guide: [`prerequisites/03_setup.md`](../prerequisites/03_setup.md)
