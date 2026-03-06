# Argo Rollouts — Progressive Delivery (Canary & Blue-Green)

**Argo Rollouts** is a Kubernetes controller that provides advanced deployment strategies like **Canary** and **Blue-Green** deployments, with automated traffic shifting and analysis.

---

## 📂 Folder Contents

| File                                               | Description                                                 |
| -------------------------------------------------- | ----------------------------------------------------------- |
| [`rollout-canary.yml`](./rollout-canary.yml)       | Canary deployment Rollout manifest (template)               |
| [`rollout-bluegreen.yml`](./rollout-bluegreen.yml) | Blue-Green deployment Rollout manifest (template)           |
| [`nginx_canary_svc.yml`](./nginx_canary_svc.yml)   | Service for canary traffic routing (template)               |
| [`svc-bluegreen.yml`](./svc-bluegreen.yml)         | Active + Preview services for blue-green routing (template) |

> **Note:** These files are templates — fill them with your application-specific configuration.

---

## 🔄 Deployment Strategies

### Canary Deployment

Gradually shifts traffic from old version to new version in increments.

```
v1 (stable) ──── 90% traffic
v2 (canary) ────  10% traffic  → 20% → 50% → 100%
```

### Blue-Green Deployment

Runs two identical environments; switches traffic all at once after verification.

```
Blue  (active)  ──── 100% traffic
Green (preview) ────   0% traffic
                      ↓ (promote)
Green (active)  ──── 100% traffic
```

---

## 🛠️ Prerequisites

1. Install Argo Rollouts controller:

```bash
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
```

2. Install the Argo Rollouts kubectl plugin:

```bash
curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-linux-amd64
chmod +x kubectl-argo-rollouts-linux-amd64
sudo mv kubectl-argo-rollouts-linux-amd64 /usr/local/bin/kubectl-argo-rollouts
```

---

## 🚀 Usage

```bash
# Apply the Rollout and Service
kubectl apply -f rollout-canary.yml
kubectl apply -f nginx_canary_svc.yml

# Monitor the rollout
kubectl argo rollouts get rollout <rollout-name> --watch

# Manually promote a canary step
kubectl argo rollouts promote <rollout-name>
```

---

## 🔗 References

- [Argo Rollouts Docs](https://argoproj.github.io/argo-rollouts/)
- [Canary Strategy](https://argoproj.github.io/argo-rollouts/features/canary/)
- [Blue-Green Strategy](https://argoproj.github.io/argo-rollouts/features/bluegreen/)
