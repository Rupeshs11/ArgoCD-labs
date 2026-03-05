# Declarative Approach with ArgoCD (Online Shop Example)

In this approach, we will deploy an application using the **Declarative method** — writing an `Application` manifest as YAML and storing it in Git.  
This is the **true GitOps way** and the recommended approach for production environments.

---

## Theory

- In the **Declarative approach**, we define the ArgoCD `Application` resource as a YAML file and store it in a Git repository.
- The Application manifest is applied to the cluster using `kubectl apply -f`, and ArgoCD takes over from there.
- This is **true GitOps** because:
  - The app definition is **version-controlled** in Git
  - Changes are **auditable** through Git history
  - The deployment is **reproducible** — anyone can clone the repo and apply
  - **Drift detection** works automatically

> ✅ This is the **recommended approach for production**.  
> It combines the power of Git-based version control with ArgoCD's continuous reconciliation.

> 💡 Unlike CLI and UI approaches, the declarative method ensures your ArgoCD application configuration is never lost — it's always in Git.

---

## Prerequisites

Before you begin, ensure you have:

1. A **Kind cluster** running
2. **ArgoCD installed & running** (via Helm or manifests)
3. **ArgoCD CLI installed** (for verification)
4. `kubectl` installed to interact with your cluster
5. A **GitHub repository** with your Kubernetes manifests

> Follow this guide to set up ArgoCD: [`prerequisites/03_setup.md`](../prerequisites/03_setup.md)

---

## Steps to Deploy Using Declarative Approach

### 1. Prepare Your Application Manifests in Git

Create a directory in your Git repo for the app manifests. Example structure:

```
declarative_approach/online-shop/
├── deployment.yml
└── service.yml
```

**`deployment.yml`** — example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: online-shop
spec:
  replicas: 2
  selector:
    matchLabels:
      app: online-shop
  template:
    metadata:
      labels:
        app: online-shop
    spec:
      containers:
        - name: online-shop
          image: nginx:latest
          ports:
            - containerPort: 80
```

**`service.yml`** — example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: online-shop-service
spec:
  selector:
    app: online-shop
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP
```

Push these files to your GitHub repository.

---

### 2. Create the ArgoCD Application Manifest

Create a file called `application.yml` (this is the ArgoCD Application CRD):

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: online-shop-app
  namespace: argocd # Application CRD always lives in argocd namespace
spec:
  project: default # ArgoCD project (use 'default' or a custom project)
  source:
    repoURL: https://github.com/<your-username>/argocd-demos.git
    targetRevision: main # Git branch to track
    path: declarative_approach/online-shop # Path in repo where K8s manifests live
  destination:
    server: https://kubernetes.default.svc # In-cluster (or use registered cluster URL)
    namespace: default # Target namespace for the app
  syncPolicy:
    automated:
      prune: true # Remove resources deleted from Git
      selfHeal: true # Auto-fix drift if resources are changed manually
    syncOptions:
      - CreateNamespace=true # Auto-create namespace if it doesn't exist
```

> **Important:** Store this `application.yml` in Git as well — this is what makes it truly declarative and GitOps-compliant.

---

### 3. Apply the Application Manifest

```bash
kubectl apply -f application.yml
```

That's it! ArgoCD will now:

1. Detect the new Application CRD
2. Connect to the Git repo
3. Fetch manifests from the specified path
4. Deploy them to the target cluster/namespace
5. Continuously watch for drift

---

### 4. Verify Application

Using ArgoCD CLI:

```bash
argocd app list
argocd app get online-shop-app
```

Using kubectl:

```bash
kubectl get applications -n argocd
kubectl get pods -n default
kubectl get svc -n default
```

You should see:

- `online-shop-app` in ArgoCD app list with **Synced** status and **Healthy** health
- Pods running (`online-shop-xxxx`)
- `online-shop-service` of type ClusterIP

---

### 5. Access the App via Browser

Port-forward the service:

```bash
kubectl port-forward svc/online-shop-service 8083:80 --address=0.0.0.0 &
```

Open inbound rule for port `8083` on your EC2 security group.

Access at:

```
http://<EC2-Public-IP>:8083
```

---

## Testing

### Make a Change in Git (Replica Scaling)

1. Open `deployment.yml`
2. Change the replicas:

```yaml
spec:
  replicas: 4
```

3. Commit & push:

```bash
git add deployment.yml
git commit -m "Scale online-shop replicas to 4"
git push origin main
```

### Observe Auto-Sync

```bash
# Check app status
argocd app get online-shop-app

# Hard refresh if needed
argocd app get online-shop-app --hard-refresh
```

ArgoCD will automatically detect the change and sync.

### Verify in Kubernetes

```bash
kubectl get pods -n default
```

Now 4 pods should be running.

---

### Test Drift Detection (Self-Heal)

Manually delete a pod to test self-healing:

```bash
kubectl delete pod <online-shop-pod-name> -n default
```

ArgoCD will detect the drift and recreate the pod automatically (because `selfHeal: true`).

---

### Test Prune (Remove from Git)

1. Delete `service.yml` from Git:

```bash
git rm declarative_approach/online-shop/service.yml
git commit -m "Remove online-shop service"
git push origin main
```

2. ArgoCD will automatically prune (delete) the service from the cluster (because `prune: true`).

---

## Key Differences — Declarative vs CLI vs UI

| Feature                  | CLI           | UI           | Declarative      |
| ------------------------ | ------------- | ------------ | ---------------- |
| **Definition stored in** | Cluster only  | Cluster only | Git + Cluster    |
| **Reproducible**         | ❌            | ❌           | ✅               |
| **Auditable**            | ❌            | ❌           | ✅ (Git history) |
| **Version controlled**   | ❌            | ❌           | ✅               |
| **True GitOps**          | ❌            | ❌           | ✅               |
| **Best for**             | Quick testing | Learning     | Production       |

---

## Common Declarative Commands

| Command                                       | Description                   |
| --------------------------------------------- | ----------------------------- |
| `kubectl apply -f application.yml`            | Create/update the Application |
| `kubectl get applications -n argocd`          | List all ArgoCD Applications  |
| `kubectl delete application <name> -n argocd` | Delete an Application         |
| `argocd app get <name>`                       | Get detailed app status       |
| `argocd app sync <name>`                      | Force sync an app             |
| `argocd app get <name> --hard-refresh`        | Force refresh from Git        |

---

## Destroy

Remove the application:

```bash
kubectl delete -f application.yml
```

Or delete the cluster entirely:

```bash
kind delete cluster --name argocd-cluster
```

---

## Wrap-Up

- You successfully deployed an app using the **Declarative approach** — the true GitOps method.
- Key takeaways:
  - The Application manifest is a **YAML file stored in Git** → version-controlled, auditable, reproducible.
  - `kubectl apply -f` is all you need — ArgoCD handles the rest.
  - Features like `selfHeal`, `prune`, and `automated` sync make this **production-ready**.
  - This is the **recommended approach** for all real-world ArgoCD deployments.
  - Combine with **App of Apps** or **ApplicationSets** to manage multiple declarative apps at scale.

Happy Learning!
