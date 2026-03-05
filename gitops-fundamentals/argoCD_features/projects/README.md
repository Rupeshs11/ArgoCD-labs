# ArgoCD Projects & RBAC

**ArgoCD Projects** (`AppProject`) provide a way to logically group Applications and enforce **access control (RBAC)**. Projects define which Git repos, clusters, and namespaces a team is allowed to use — essential for multi-team organizations.

---

## 📂 Folder Contents

| File                           | Description                                                                   |
| ------------------------------ | ----------------------------------------------------------------------------- |
| [`project.yml`](./project.yml) | AppProject definition for `frontend-team` with RBAC roles and policies        |
| [`nginx.yml`](./nginx.yml)     | Application scoped to the `frontend-team` project with `CreateNamespace=true` |

---

## 🧠 How Projects Work

```
              ArgoCD
                │
     ┌──────────┼──────────┐
     ▼                      ▼
┌──────────────┐    ┌──────────────┐
│ frontend-team│    │ backend-team │  ← Projects
│   Project    │    │   Project    │
├──────────────┤    ├──────────────┤
│ Allowed Repos│    │ Allowed Repos│
│ Allowed NS   │    │ Allowed NS   │
│ Allowed Cluster│  │ Allowed Cluster│
│ RBAC Roles   │    │ RBAC Roles   │
└──────┬───────┘    └──────┬───────┘
       │                    │
  ┌────┴────┐          ┌───┴────┐
  │nginx-app│          │api-app │  ← Applications
  └─────────┘          └────────┘
```

---

## 📄 Key Fields in `project.yml`

| Field                        | Purpose                                            |
| ---------------------------- | -------------------------------------------------- |
| `sourceRepos`                | Git repos allowed for apps in this project         |
| `destinations`               | Allowed cluster + namespace combinations           |
| `clusterResourceWhitelist`   | Cluster-scoped resources the project can create    |
| `namespaceResourceWhitelist` | Namespace-scoped resources the project can create  |
| `roles`                      | RBAC roles with policies (e.g., `frontend-admins`) |

### RBAC Policy Syntax

```
p, proj:<project>:<role>, applications, <action>, <project>/<app-pattern>, <allow|deny>
```

Example from `project.yml`:

```
p, proj:frontend-team:frontend-admins, applications, *, frontend-team/*, allow
```

→ `frontend-admins` can perform **all actions** on **all apps** in the `frontend-team` project.

---

## 🛠️ Usage

### Step 1: Create the Project

```bash
kubectl apply -f project.yml
```

### Step 2: Verify

```bash
argocd proj list
argocd proj get frontend-team
```

### Step 3: Deploy an Application Under This Project

```bash
kubectl apply -f nginx.yml
```

### Step 4: Verify App

```bash
argocd app get nginx-frontend
argocd app list
```

---

## 📍 Required Paths & Configuration

| Config                              | Purpose                                                                |
| ----------------------------------- | ---------------------------------------------------------------------- |
| `sourceRepos`                       | Git repos allowed (e.g., `https://github.com/<user>/argocd-demos.git`) |
| `destinations.server`               | Cluster URL (from `argocd cluster list`)                               |
| `destinations.namespace`            | Namespace the project's apps can deploy to (e.g., `frontend`)          |
| `project` field in Application      | Must match the AppProject name (e.g., `frontend-team`)                 |
| `syncOptions: CreateNamespace=true` | Auto-creates the namespace if it doesn't exist                         |

---

## ✅ Best Practices

- Create **one project per team** (e.g., `frontend-team`, `backend-team`, `platform`)
- Restrict `sourceRepos` to only the repos the team owns
- Restrict `destinations` to the team's namespaces and clusters
- Use RBAC roles to control who can sync, delete, or override apps
- Always set `CreateNamespace=true` if the target namespace might not exist

---

## ➡️ Next Steps

- Combine with [`ApplicationSets/`](../ApplicationSets/) to auto-generate project-scoped apps at scale
- Explore [`multi-cluster/`](../multi-cluster/) for cross-cluster deployments with project restrictions
