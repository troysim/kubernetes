# 🚀 Hello App on Kubernetes with Kind and Ingress

This project sets up a local Kubernetes environment using Kind (Kubernetes in Docker) and deploys a simple HTTP echo application.

It demonstrates how to:

1. Create a local Kubernetes cluster using Kind with ports **80 and 443 mapped to localhost** — allowing access via browser or `curl`.
2. Deploy a lightweight HTTP echo service using the [`hashicorp/http-echo`](https://hub.docker.com/r/hashicorp/http-echo) container.
3. Expose the service internally via a standard Kubernetes `Service`.
4. Expose the service externally using an `Ingress` with the **NGINX Ingress Controller**, making it accessible at:

   ```
   http://localhost/hello
   ```

5. Enable easy local testing of Ingress routes without needing cloud services, TLS certificates, or DNS — ideal for:
   - Local development and debugging
   - Learning Kubernetes Ingress concepts
   - Demonstrating microservice exposure through Ingress

---

## 📦 What’s Included

| File               | Description                                      |
|--------------------|--------------------------------------------------|
| `kind-config.yaml` | Custom Kind config with port mappings (80/443)  |
| `hello-app.yaml`   | Deployment + Service for the echo container      |
| `hello-ingress.yaml` | Ingress resource for the `/hello` path         |

---

## ✅ Prerequisites

Make sure the following tools are installed:

- [Docker](https://www.docker.com/)
- [`kind`](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- [`kubectl`](https://kubernetes.io/docs/tasks/tools/)

---

## 🛠 Setup Instructions

### 1. Create the Kind Cluster

```bash
kind create cluster --config kind-config.yaml
```

> This maps port 80 and 443 from your host to the Kind node.

---

### 2. Deploy NGINX Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/kind/deploy.yaml
kubectl label node kind-control-plane ingress-ready=true
```

---

### 3. Deploy the Application

```bash
kubectl apply -f hello-app.yaml
```

---

### 4. Deploy the Ingress Resource

```bash
kubectl apply -f hello-ingress.yaml
```

---

## 🌐 Test the Setup

Once everything is deployed, test the endpoint:

```bash
curl http://localhost/hello
```

You should see:

```
Hello from Kind!
```

---

## 🧪 Troubleshooting

- Check if the ingress controller is running:
  ```bash
  kubectl get pods -n ingress-nginx
  ```

- Describe the ingress resource:
  ```bash
  kubectl describe ingress hello-ingress
  ```

- View services:
  ```bash
  kubectl get svc
  ```

---

## 🧭 What This Project Does (Summary)

- Creates a local Kind cluster with port mappings
- Deploys a simple HTTP echo container
- Exposes the app using Kubernetes `Ingress` + NGINX
- Provides a clean way to test microservice routing locally
