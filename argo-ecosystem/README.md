# Argo Ecosystem

This folder covers the **Argo Project ecosystem** beyond ArgoCD — additional Kubernetes-native tools for workflows, event-driven automation, and progressive delivery.

---

## 📂 Folder Structure

```
argo-ecosystem/
├── argo_workflow/         # Argo Workflows — Kubernetes-native workflow engine
│   ├── topic.md           # Theory: what, why, architecture, template types
│   ├── installation.sh    # CLI installation script
│   └── manifest/          # Example workflow manifests
│       ├── hello-world.yml
│       ├── cicd.yml
│       ├── cron-workflow.yml
│       ├── prefix.yml
│       └── workflow-template.yml
│
├── argo_events/           # Argo Events — Event-driven workflow triggers
│   ├── EventSource.yml    # Webhook EventSource configuration
│   ├── webhook-sensors.yml # Sensor that triggers workflows on events
│   └── installation.sh    # CLI installation script
│
└── argo_rollouts/         # Argo Rollouts — Progressive delivery
    ├── rollout-canary.yml       # Canary deployment strategy
    ├── rollout-bluegreen.yml    # Blue-Green deployment strategy
    ├── nginx_canary_svc.yml     # Service for canary routing
    └── svc-bluegreen.yml        # Services for blue-green routing
```

---

## 🔄 Ecosystem Overview

| Tool               | Purpose                           | Use Case                                                |
| ------------------ | --------------------------------- | ------------------------------------------------------- |
| **Argo Workflows** | Kubernetes-native workflow engine | CI/CD pipelines, ML workflows, batch jobs               |
| **Argo Events**    | Event-driven automation           | Trigger workflows on webhooks, Git events, S3 uploads   |
| **Argo Rollouts**  | Progressive delivery controller   | Canary & Blue-Green deployments with traffic management |

---

## 📖 Learning Order

1. **Argo Workflows** → [`argo_workflow/`](./argo_workflow/) — Understand workflow orchestration
2. **Argo Events** → [`argo_events/`](./argo_events/) — Event-driven triggers for workflows
3. **Argo Rollouts** → [`argo_rollouts/`](./argo_rollouts/) — Advanced deployment strategies

---

## 🔗 Official Docs

| Tool           | Documentation                                                                 |
| -------------- | ----------------------------------------------------------------------------- |
| Argo Workflows | [argo-workflows.readthedocs.io](https://argo-workflows.readthedocs.io/)       |
| Argo Events    | [argoproj.github.io/argo-events](https://argoproj.github.io/argo-events/)     |
| Argo Rollouts  | [argoproj.github.io/argo-rollouts](https://argoproj.github.io/argo-rollouts/) |

---

## ➡️ Related

- ArgoCD Fundamentals: [`gitops-fundamentals/`](../gitops-fundamentals/)
- ArgoCD Advanced: [`gitops-advanced/`](../gitops-advanced/)
- Full project: [github.com/believer-11/argocd-demos](https://github.com/believer-11/argocd-demos)
