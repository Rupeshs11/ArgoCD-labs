# Argo Events — Event-Driven Workflow Automation

**Argo Events** is an event-driven framework for Kubernetes that connects event sources (webhooks, Git, S3, etc.) to triggers (Argo Workflows, Kubernetes resources, etc.). It allows you to automate tasks based on real-time events.

---

## 📂 Folder Contents

| File                                           | Description                                                      |
| ---------------------------------------------- | ---------------------------------------------------------------- |
| [`EventSource.yml`](./EventSource.yml)         | Webhook EventSource — exposes an HTTP endpoint to receive events |
| [`webhook-sensors.yml`](./webhook-sensors.yml) | Sensor — listens to EventSource and triggers an Argo Workflow    |
| [`installation.sh`](./installation.sh)         | Argo CLI installation script (Linux/macOS)                       |

---

## 🧠 How It Works

```
External Event (webhook/Git/S3)
        │
        ▼
┌──────────────────┐
│   EventSource    │ ← Receives events via HTTP/webhook
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│     Sensor       │ ← Filters events and triggers actions
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Argo Workflow   │ ← Executes the triggered workflow
└──────────────────┘
```

---

## 🛠️ Usage

### Install Argo Events

```bash
kubectl create namespace argo-events
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml
```

### Apply the EventSource and Sensor

```bash
kubectl apply -f EventSource.yml
kubectl apply -f webhook-sensors.yml
```

### Test the Webhook

```bash
curl -X POST -d '{"message": "hello"}' http://<event-source-svc>:12000/example
```

---

## 🔗 References

- [Argo Events Docs](https://argoproj.github.io/argo-events/)
- Argo Workflows: [`argo_workflow/`](../argo_workflow/)
