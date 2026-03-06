# Argo Workflows — Kubernetes-Native Workflow Engine

**Argo Workflows** is an open-source, container-native workflow engine for orchestrating parallel jobs on Kubernetes. Each task runs as a Pod, providing native scaling, scheduling, and isolation.

---

## 📂 Folder Contents

| File                                   | Description                                                               |
| -------------------------------------- | ------------------------------------------------------------------------- |
| [`topic.md`](./topic.md)               | Theory — what is Argo Workflows, why use it, architecture, template types |
| [`installation.sh`](./installation.sh) | Argo CLI installation script (Linux/macOS)                                |
| [`manifest/`](./manifest/)             | Example workflow manifests                                                |

### `manifest/` Directory

| File                                                        | Description                                              |
| ----------------------------------------------------------- | -------------------------------------------------------- |
| [`hello-world.yml`](./manifest/hello-world.yml)             | Multi-step workflow with parameter passing between steps |
| [`cicd.yml`](./manifest/cicd.yml)                           | Simple CI/CD pipeline (Build → Test → Deploy)            |
| [`cron-workflow.yml`](./manifest/cron-workflow.yml)         | CronWorkflow — scheduled job (runs every minute)         |
| [`prefix.yml`](./manifest/prefix.yml)                       | CI/CD pipeline with `generateName` prefix                |
| [`workflow-template.yml`](./manifest/workflow-template.yml) | Reusable WorkflowTemplate with input parameters          |

---

## 🛠️ Setup

### Install Argo Workflows

```bash
kubectl create namespace argo
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.7.0/quick-start-minimal.yaml
```

### Install Argo CLI

```bash
chmod +x installation.sh
./installation.sh
```

---

## 🚀 Usage

```bash
# Submit a workflow
argo submit manifest/hello-world.yml -n argo --watch

# List workflows
argo list -n argo

# Get workflow details
argo get <workflow-name> -n argo

# View logs
argo logs <workflow-name> -n argo

# Access Argo UI
kubectl -n argo port-forward svc/argo-server 2746:2746 &
```

---

## 📖 Learning Order

1. Read [`topic.md`](./topic.md) for theory and architecture
2. Try [`hello-world.yml`](./manifest/hello-world.yml) — basic multi-step workflow
3. Try [`cicd.yml`](./manifest/cicd.yml) — CI/CD pipeline pattern
4. Try [`cron-workflow.yml`](./manifest/cron-workflow.yml) — scheduled jobs
5. Try [`workflow-template.yml`](./manifest/workflow-template.yml) — reusable templates

---

## 🔗 References

- [Argo Workflows Docs](https://argo-workflows.readthedocs.io/)
- Argo Events: [`argo_events/`](../argo_events/)
