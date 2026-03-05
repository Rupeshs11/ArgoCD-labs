# ArgoCD Approaches — Three Ways to Deploy Applications

This folder covers the **three primary methods** of deploying applications with ArgoCD. Each approach has its own use case and trade-offs.

---

## 📂 Folder Contents

| File                                                   | Description                                                                           |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| [`cli_approach.md`](./cli_approach.md)                 | Deploy an app using `argocd` CLI commands (Apache example)                            |
| [`UI_approach.md`](./UI_approach.md)                   | Deploy an app through ArgoCD Web Dashboard (Nginx example)                            |
| [`declarative_approach.md`](./declarative_approach.md) | Deploy using `Application` YAML manifests — the true GitOps way (Online Shop example) |
| `outputs/`                                             | Directory for output screenshots                                                      |

---

## 🔄 Comparison of Approaches

| Feature                | CLI Approach                | UI Approach               | Declarative Approach               |
| ---------------------- | --------------------------- | ------------------------- | ---------------------------------- |
| **How**                | `argocd app create` command | Point-and-click in web UI | `kubectl apply -f application.yml` |
| **Speed**              | Fast for operators          | Fastest for beginners     | Requires YAML knowledge            |
| **GitOps Compliant**   | ❌ No (imperative)          | ❌ No (imperative)        | ✅ Yes (true GitOps)               |
| **Version Controlled** | ❌ Not in Git               | ❌ Not in Git             | ✅ Stored in Git                   |
| **Best For**           | Admins, quick testing       | Learning, visualization   | Production, teams                  |
| **Reproducible**       | Manual re-run needed        | Manual re-creation        | `git clone` + `kubectl apply`      |

---

## 📖 Recommended Order

1. **CLI Approach** → Understand how ArgoCD works from the terminal
2. **UI Approach** → Visualize the same workflow through the web dashboard
3. **Declarative Approach** → Learn the production-ready GitOps method

---

## 🛠️ Prerequisites

Before starting any approach, ensure you have:

- A **Kind cluster** running
- **ArgoCD installed & running** (via Helm or Manifests)
- **ArgoCD CLI installed**
- **kubectl** configured

> Follow the setup guide: [`prerequisites/03_setup.md`](../prerequisites/03_setup.md)

---

## ➡️ Next Steps

After completing all three approaches, explore advanced ArgoCD features in [`argoCD_features/`](../argoCD_features/).
