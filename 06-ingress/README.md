# Kubernetes Ingress

---

# 📖 Introduction

Ingress is a Kubernetes API object that manages external HTTP and HTTPS traffic to applications running inside a Kubernetes cluster.

Instead of exposing every application using a separate LoadBalancer or NodePort Service, Ingress provides a single entry point and routes incoming requests to the appropriate Service based on hostnames or URL paths.

---

# 🎯 Objectives

- Understand why Ingress is required
- Learn the difference between NodePort and Ingress
- Configure multiple applications behind a single Ingress
- Route traffic using hostname-based routing
- Understand the complete request flow

---

# 🏗 Architecture

```
                Browser
                    │
                    ▼
         NGINX Ingress Controller
                    │
             Ingress Rules
           ┌────────┴────────┐
           │                 │
           ▼                 ▼
    nginx-service     apache-service
           │                 │
           ▼                 ▼
      nginx Pod         apache Pod
```

---

# 📂 YAML Files

This practical contains the following Kubernetes manifests.

```
nginx-deployment.yaml
apache-deployment.yaml
nginx-service.yaml
apache-service.yaml
ingress.yaml
```

---

# 💻 Commands Used

Enable the Ingress Controller

```bash
minikube addons enable ingress
```

Verify the controller

```bash
kubectl get pods -n ingress-nginx
```

Deploy the applications

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f apache-deployment.yaml
```

Create the Services

```bash
kubectl apply -f nginx-service.yaml
kubectl apply -f apache-service.yaml
```

Create the Ingress

```bash
kubectl apply -f ingress.yaml
```

Verify

```bash
kubectl get ingress
```

Describe the Ingress

```bash
kubectl describe ingress apps-ingress
```

Verify routing

```bash
curl -H "Host: nginx.local" http://$(minikube ip)

curl -H "Host: apache.local" http://$(minikube ip)
```

---

# 🌐 Request Flow

```
Browser

↓

Ingress Controller

↓

Ingress Rules

↓

ClusterIP Service

↓

Endpoints

↓

Pod
```

---

# 🧠 Concepts Learned

- Ingress does not directly communicate with Pods.
- Ingress routes traffic to Kubernetes Services.
- Services route traffic to healthy Pods.
- Pods are ephemeral and may change after a restart.
- Services provide a stable ClusterIP.
- Endpoints store the IP addresses of the running Pods.
- One Ingress can expose multiple applications.

---

# 🎤 Interview Questions

### What is Ingress?

Ingress is a Kubernetes resource that manages external HTTP/HTTPS traffic and routes requests to Services using host or path-based rules.

---

### Why doesn't Ingress send traffic directly to Pods?

Pods are ephemeral and their IP addresses change when they are recreated.

Services provide a stable endpoint and automatically track healthy Pods using Endpoints.

---

### What is the role of the Ingress Controller?

The Ingress Controller watches Ingress resources and configures the underlying proxy (NGINX in this practical) to route incoming traffic.

---

### Difference between NodePort and Ingress

| NodePort | Ingress |
|----------|----------|
| Exposes one Service | Can expose multiple Services |
| Uses ports 30000-32767 | Uses HTTP/HTTPS (80/443) |
| No intelligent routing | Host and path-based routing |
| Layer 4 | Layer 7 |

---

# ⚠ Troubleshooting

### Browser could not reach nginx.local

Reason:

Windows browser could not access the Minikube Docker network while using WSL2.

Verification:

```bash
curl -H "Host: nginx.local" http://$(minikube ip)

curl -H "Host: apache.local" http://$(minikube ip)
```

The successful curl responses confirmed that the Ingress configuration was working correctly.

---

# 📸 Screenshots

The following screenshots will be added later.

- kubectl get pods
- kubectl get deploy
- kubectl get svc
- kubectl get ingress
- kubectl describe ingress
- Browser output
- curl output

---

# ✅ Summary

In this practical, I deployed two applications (Nginx and Apache), exposed them internally using ClusterIP Services, and configured a single NGINX Ingress to route requests based on hostnames.

This demonstrated how Ingress acts as a single entry point while Services provide stable networking to dynamically changing Pods.
