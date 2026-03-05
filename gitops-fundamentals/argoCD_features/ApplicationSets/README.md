# ApplicationSets — Auto-Generate Applications at Scale

**ApplicationSets** allow you to automatically generate multiple ArgoCD Applications from a single template using **generators**. Instead of writing one Application YAML per app/cluster, a single ApplicationSet can create dozens of apps dynamically.

---

## 📂 Folder Contents

| File                                               | Description                                                           |
| -------------------------------------------------- | --------------------------------------------------------------------- |
| [`list_generator.yml`](./list_generator.yml)       | Generates apps from a static list of elements (app name + path pairs) |
| [`git_generator.yml`](./git_generator.yml)         | Scans a Git repo and generates one app per matching directory         |
| [`cluster_generator.yml`](./cluster_generator.yml) | Generates one app per registered ArgoCD cluster                       |

---

## 🔄 Generator Types Explained

### 1. List Generator (`list_generator.yml`)

Manually define a list of apps with their paths. ArgoCD generates one Application per list element.

```yaml
generators:
  - list:
      elements:
        - app: nginx
          path: ui_approach/nginx
        - app: online-shop
          path: multicluster/online-shop
```

**Use when:** You have a known, fixed set of apps to deploy.

---

### 2. Git Generator (`git_generator.yml`)

Scans a Git repo directory and creates one Application for each sub-directory matching a glob pattern.

```yaml
generators:
  - git:
      repoURL: https://github.com/beleiver-11/argocd-demos.git
      revision: main
      directories:
        - path: "git_generator/*"
```

**Use when:** You have a monorepo where each app lives in its own folder.

---

### 3. Cluster Generator (`cluster_generator.yml`)

Iterates over all clusters registered in ArgoCD and deploys the same app to each one.

```yaml
generators:
  - clusters: {} # empty = all registered clusters
```

**Use when:** You want to deploy the same app (e.g., monitoring, logging) to all clusters.

---

## 🛠️ Usage

### Apply an ApplicationSet

```bash
kubectl apply -f list_generator.yml
# or
kubectl apply -f git_generator.yml
# or
kubectl apply -f cluster_generator.yml
```

### Verify Generated Apps

```bash
argocd app list
```

### Delete an ApplicationSet

```bash
kubectl delete applicationset <name> -n argocd
```

> Deleting the ApplicationSet will also delete all the Applications it generated.

---

## 📍 Required Paths

| Path / Config           | Purpose                                                   |
| ----------------------- | --------------------------------------------------------- |
| `repoURL`               | Your Git repository URL containing Kubernetes manifests   |
| `path` / `directories`  | Path patterns in the repo where app manifests are located |
| `server` in destination | Target cluster URL (from `argocd cluster list`)           |
| `argocd` namespace      | Where the ApplicationSet CRD is created                   |

---

## ✅ Best Practices

- Use **Git generator** for monorepos — it auto-discovers new apps when you add folders
- Use **Cluster generator** for cross-cluster tools (monitoring, ingress, cert-manager)
- Use **List generator** for a small, well-defined set of apps
- Always enable `prune: true` and `selfHeal: true` in the template
- Combine generators with **Projects** for team-level access control

---

## ➡️ Next Steps

- Explore [`multi-cluster/`](../multi-cluster/) for manual multi-cluster deployments
- Learn [`projects/`](../projects/) to restrict team-level access
