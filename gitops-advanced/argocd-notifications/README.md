# ArgoCD Notifications — Email Alerts

This folder contains template manifests for setting up **ArgoCD Notifications** with **email (SMTP)** alerts. Get notified when applications are deployed or become unhealthy.

---

## 📂 Folder Contents

| File                                                         | Description                                               |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| [`app.yml`](./app.yml)                                       | ArgoCD Application template with notification annotations |
| [`notification-configmap.yml`](./notification-configmap.yml) | ConfigMap with SMTP config, email templates, and triggers |
| [`smtp-secrets.yml`](./smtp-secrets.yml)                     | Kubernetes Secret for SMTP credentials                    |

---

## 🧠 How It Works

```
App State Changes
       │
       ▼
┌──────────────────────┐
│ Notification Triggers │ ← Watches app health/sync status
├──────────────────────┤
│ on-health-degraded   │ → Fires when health = Degraded
│ on-deployed          │ → Fires when sync succeeded + healthy
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│   Email Templates    │ ← Formats the notification message
└──────────┬───────────┘
           │
           ▼
       📧 Email Sent
```

---

## 🛠️ Setup Steps

### 1. Create the SMTP Secret

Update `smtp-secrets.yml` with your SMTP credentials, then apply:

```bash
kubectl apply -f smtp-secrets.yml
```

### 2. Apply the Notification ConfigMap

```bash
kubectl apply -f notification-configmap.yml
```

### 3. Add Notification Annotations to Your App

Add these annotations to any Application you want to monitor (see `app.yml` for template):

```yaml
annotations:
  notifications.argoproj.io/subscribe.on-health-degraded.email: "receiver@example.com"
  notifications.argoproj.io/subscribe.on-deployed.email: "receiver@example.com"
```

---

## 📍 Configuration Notes

| Config              | File                         | What to Replace                             |
| ------------------- | ---------------------------- | ------------------------------------------- |
| SMTP email/password | `smtp-secrets.yml`           | Your SMTP credentials                       |
| ArgoCD server URL   | `notification-configmap.yml` | `<your-argocd-server>` with actual URL      |
| Receiver emails     | `app.yml` annotations        | `<receiver@example.com>` with actual emails |
| Git repo URL        | `app.yml` spec               | `<your-username>` with your GitHub username |

---

## ➡️ Related

- Monitoring: [`Monitoring/`](../Monitoring/)
- Image Updater: [`argocd_imageUpdater/`](../argocd_imageUpdater/)
