# UI Approach with ArgoCD (Nginx Example)

In this approach, we will deploy an application using the **ArgoCD Web UI (Dashboard)**.  
We'll use a simple **Nginx Deployment + Service** to understand how ArgoCD can be operated through its graphical interface.

---

## Theory

- In the **UI approach**, we use the ArgoCD **web dashboard** to visually create, manage, and sync applications.
- Behind the scenes, the UI creates the same `Application` CRD in the cluster — it's just a visual interface over the API.
- The app definition lives **only in the cluster**, not in Git.
- It's the most beginner-friendly way to learn ArgoCD, but still **not true GitOps**, because changes are not version-controlled.

> ⚠️ Important: The ArgoCD **Server** must be running and accessible via browser.  
> The UI is just a client that interacts with the ArgoCD API server.

> ✅ Best practice: Use the **Declarative approach (CRDs in Git)** for production.  
> ❌ The UI method is best for learning and quick visualization.

---

## Prerequisites

Before you begin, ensure you have:

1. A **Kind cluster** running
2. **ArgoCD installed & running** (via Helm or manifests)
3. **ArgoCD UI accessible** via port-forward
4. `kubectl` installed to interact with your cluster

> Follow this guide to set up ArgoCD: [`prerequisites/03_setup.md`](../prerequisites/03_setup.md)

---

## Steps to Deploy Nginx using ArgoCD UI

### 1. Access the ArgoCD Dashboard

Port-forward the ArgoCD server:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443 --address=0.0.0.0 &
```

Open your browser and navigate to:

```
https://<instance_public_ip>:8080
```

Login credentials:

- **Username:** `admin`
- **Password:** Retrieve using:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd \
  -o jsonpath="{.data.password}" | base64 -d && echo
```

---

### 2. Prepare Your Git Repository

Make sure your Git repo has the Nginx manifests. Example structure:

```
ui_approach/nginx/
├── nginx_deployment.yml
└── nginx_svc.yml
```

**`nginx_deployment.yml`** — example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

**`nginx_svc.yml`** — example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP
```

Push these files to your GitHub repository.

---

### 3. Create Application via ArgoCD UI

1. Click **"+ NEW APP"** button in the ArgoCD dashboard

2. Fill in the **General** section:
   - **Application Name:** `nginx-app`
   - **Project Name:** `default`
   - **Sync Policy:** `Automatic`
   - **Check:** ✅ `AUTO-CREATE NAMESPACE`
   - **Check:** ✅ `SELF HEAL`
   - **Check:** ✅ `PRUNE`

3. Fill in the **Source** section:
   - **Repository URL:** `https://github.com/<your-username>/argocd-demos.git`
   - **Revision:** `main` (or `HEAD`)
   - **Path:** `ui_approach/nginx`

4. Fill in the **Destination** section:
   - **Cluster URL:** `https://kubernetes.default.svc` (in-cluster) or select your registered cluster
   - **Namespace:** `default`

5. Click **"CREATE"**

---

### 4. Observe the Application

After creation, ArgoCD will:

1. Show the application tile on the dashboard
2. Begin syncing automatically (if auto-sync is enabled)
3. Display the **sync status** and **health status**

Click on the application tile to see:

- **Resource tree** — visual map of deployed resources (Deployment, ReplicaSet, Pods, Service)
- **Sync status** — Synced / OutOfSync
- **Health status** — Healthy / Degraded / Progressing

---

### 5. Verify Deployment in Kubernetes

```bash
kubectl get pods -n default
kubectl get svc -n default
```

You should see:

- Nginx pods running (`nginx-deployment-xxxx`)
- `nginx-service` of type ClusterIP exposing port 80

---

### 6. Access Nginx via Browser

Port-forward the Nginx service:

```bash
kubectl port-forward svc/nginx-service 8081:80 --address=0.0.0.0 &
```

Open inbound rule for port `8081` on your EC2 security group.

Access at:

```
http://<EC2-Public-IP>:8081
```

You should see the default **Nginx welcome page**.

---

## Testing

### Make a Change in Git

1. Open `nginx_deployment.yml`
2. Change the replicas, e.g., `2 → 3`:

```yaml
spec:
  replicas: 3
```

3. Commit & push:

```bash
git add nginx_deployment.yml
git commit -m "Scale Nginx replicas to 3"
git push origin main
```

### Observe in ArgoCD UI

1. Go back to the ArgoCD dashboard
2. The app will briefly show **OutOfSync**
3. ArgoCD will automatically sync (since auto-sync is enabled)
4. The resource tree will update to show 3 pods

### Verify in Kubernetes

```bash
kubectl get pods -n default
```

Now 3 Nginx pods should be running.

---

## Common UI Operations

| Operation        | How to Do It                                              |
| ---------------- | --------------------------------------------------------- |
| **Create App**   | Click "+ NEW APP" → fill form → "CREATE"                  |
| **Sync App**     | Click app tile → "SYNC" button                            |
| **Refresh App**  | Click app tile → "REFRESH" button                         |
| **Delete App**   | Click app tile → "DELETE" button                          |
| **View Logs**    | Click on a Pod in the resource tree → "LOGS" tab          |
| **View Events**  | Click on any resource → "EVENTS" tab                      |
| **Rollback**     | Click app tile → "HISTORY AND ROLLBACK" → select revision |
| **Hard Refresh** | Click "REFRESH" dropdown → "HARD REFRESH"                 |

---

## Destroy

To remove the application:

1. In ArgoCD UI: Click the app tile → **DELETE** → confirm
2. Or delete the cluster entirely:

```bash
kind delete cluster --name argocd-cluster
```

---

## Wrap-Up

- You successfully deployed an app via **ArgoCD Web UI**.
- Key takeaways:
  - The UI is the most **visual and beginner-friendly** way to use ArgoCD.
  - It creates the same Application CRD as CLI/declarative methods.
  - The resource tree visualization is **excellent for debugging** and understanding app architecture.
  - For production, always use the **Declarative approach** with manifests stored in Git.

Happy Learning!
