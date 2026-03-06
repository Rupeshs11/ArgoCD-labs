# ArgoCD SSL/TLS — Ingress + Let's Encrypt Setup

This folder contains template manifests for exposing ArgoCD over **HTTPS** using **Nginx Ingress** and **cert-manager** with **Let's Encrypt** certificates.

---

## 📂 Folder Contents

| File                                                 | Description                                                   |
| ---------------------------------------------------- | ------------------------------------------------------------- |
| [`argocd-ingress.yml`](./argocd-ingress.yml)         | Nginx Ingress resource for ArgoCD server with SSL passthrough |
| [`letsencrypt-issuer.yml`](./letsencrypt-issuer.yml) | ClusterIssuer for Let's Encrypt (production + staging)        |

---

## 🛠️ Prerequisites

Before applying these manifests, ensure:

1. **Nginx Ingress Controller** is installed in your cluster
2. **cert-manager** is installed ([Install Guide](https://cert-manager.io/docs/installation/))
3. **A domain name** pointing to your cluster's load balancer IP
4. **ArgoCD** is installed in the `argocd` namespace

---

## 📄 Configuration

### `argocd-ingress.yml`

| Field                            | Value                   | Notes                                |
| -------------------------------- | ----------------------- | ------------------------------------ |
| `host`                           | `argocd.yourdomain.com` | **Replace** with your actual domain  |
| `cert-manager.io/cluster-issuer` | `letsencrypt-prod`      | References the ClusterIssuer         |
| `ssl-passthrough`                | `true`                  | Forwards HTTPS directly to ArgoCD    |
| `secretName`                     | `argocd-server-tls`     | TLS cert stored here by cert-manager |

### `letsencrypt-issuer.yml`

| Field              | Value                                  | Notes                                              |
| ------------------ | -------------------------------------- | -------------------------------------------------- |
| `email`            | `<your-email@example.com>`             | **Replace** with your email for cert notifications |
| `server` (prod)    | `acme-v02.api.letsencrypt.org`         | Trusted production certificates                    |
| `server` (staging) | `acme-staging-v02.api.letsencrypt.org` | For testing (untrusted certs)                      |

---

## 🚀 Usage

```bash
# 1. Apply the ClusterIssuer first
kubectl apply -f letsencrypt-issuer.yml

# 2. Apply the Ingress
kubectl apply -f argocd-ingress.yml

# 3. Verify certificate issuance
kubectl get certificate -n argocd
kubectl describe certificate argocd-server-tls -n argocd
```

Access ArgoCD at: `https://argocd.yourdomain.com`

---

## ➡️ Related

- ArgoCD Setup: [`gitops-fundamentals/prerequisites/`](../gitops-fundamentals/prerequisites/)
- Full project: [github.com/believer-11/argocd-demos](https://github.com/believer-11/argocd-demos)
