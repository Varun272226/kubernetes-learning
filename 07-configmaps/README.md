# Kubernetes ConfigMaps

---

## 📖 Introduction

A ConfigMap is a Kubernetes API object used to store non-sensitive configuration data as key-value pairs. It allows configuration to be separated from the application code and Docker image, making applications more portable and easier to manage across different environments.

Instead of rebuilding a Docker image whenever a configuration changes, Kubernetes allows us to update the ConfigMap and provide the configuration to Pods.

---

## 🎯 Objectives

- Understand why ConfigMaps are required.
- Learn how to create a ConfigMap.
- Inject configuration into Pods using environment variables.
- Verify configuration inside a running container.
- Understand how ConfigMap updates affect running Pods.
- Learn the difference between ConfigMaps and Secrets.

---

## ❓ Why do we need ConfigMaps?

Imagine an application that connects to a database.

```
DATABASE_HOST=mysql.company.com
LOG_LEVEL=INFO
APP_ENV=Production
```

If these values are hardcoded inside the application, every configuration change requires:

- Updating the application code.
- Building a new Docker image.
- Pushing the image to a container registry.
- Redeploying the application.

This is inefficient.

ConfigMaps solve this problem by storing configuration separately from the application.

This follows one of the core DevOps principles:

> **Build once, deploy everywhere.**

The same Docker image can be deployed in Development, Testing, and Production while using different ConfigMaps for each environment.

---

## 🏗️ Architecture

```
                ConfigMap
                     │
                     │
     APP_ENV=Production
     DATABASE_HOST=mysql.company.com
     LOG_LEVEL=INFO
                     │
                     ▼
               Deployment
                     │
                     ▼
                    Pod
                     │
                     ▼
                 Container
                     │
                     ▼
          Environment Variables
```

---

## 📂 Files

```
configmap.yaml
deployment.yaml
README.md
```

---

## 💻 Commands Used

Create the ConfigMap

```bash
kubectl apply -f configmap.yaml
```

Verify the ConfigMap

```bash
kubectl get configmaps

kubectl describe configmap app-config
```

Deploy the application

```bash
kubectl apply -f deployment.yaml
```

Verify the Deployment

```bash
kubectl get deploy
```

Verify the Pod

```bash
kubectl get pods
```

Check environment variables inside the Pod

```bash
kubectl exec -it <pod-name> -- env
```

Filter individual variables

```bash
kubectl exec -it <pod-name> -- env | grep APP

kubectl exec -it <pod-name> -- env | grep DATABASE

kubectl exec -it <pod-name> -- env | grep LOG
```

Restart the Deployment

```bash
kubectl rollout restart deployment configmap-demo
```

---

## 🔄 Request Flow

```
ConfigMap

↓

Deployment

↓

Pod

↓

Container

↓

Environment Variables
```

---

## 🧠 Concepts Learned

- ConfigMaps store non-sensitive configuration.
- Configuration is separated from the Docker image.
- Applications become reusable across multiple environments.
- ConfigMaps can inject values into containers as environment variables.
- Running Pods do not automatically receive updated ConfigMap values when environment variables are used.
- Pods must be restarted to read updated environment variables.
- ConfigMaps should not store passwords or API keys.

---

## 🎤 Interview Questions

### What is a ConfigMap?

A ConfigMap is a Kubernetes resource used to store non-sensitive configuration as key-value pairs. It allows configuration to be managed separately from the application container.

---

### Why do we use ConfigMaps?

ConfigMaps allow configuration to be updated without rebuilding the Docker image, making applications portable across multiple environments.

---

### What type of data should be stored in a ConfigMap?

Examples include:

- Database host
- Application environment
- Log level
- Feature flags
- Application configuration

Sensitive information such as passwords, tokens, and API keys should not be stored in ConfigMaps.

---

### What happens when a ConfigMap is updated?

When ConfigMaps are used as environment variables:

- Running Pods continue using the old values.
- Pods must be restarted to load the updated configuration.

---

### Difference between ConfigMap and Secret

| ConfigMap | Secret |
|-----------|--------|
| Stores non-sensitive configuration | Stores sensitive information |
| Plain text | Base64 encoded (not encrypted by default) |
| Database host, log level | Passwords, API keys, tokens |

---

## ⚠️ Troubleshooting

### ConfigMap key not found

Cause:

The Deployment references a key that does not exist in the ConfigMap.

Example:

```yaml
key: DATABASE
```

while the ConfigMap contains:

```yaml
DATABASE_HOST
```

Solution:

Ensure the key names match exactly.

---

### Updated ConfigMap but Pod still shows old values

Cause:

Environment variables are loaded only when the container starts.

Solution:

Restart the Deployment.

```bash
kubectl rollout restart deployment configmap-demo
```

---

## 📸 Screenshots

Screenshots will be added later.

- ConfigMap creation
- kubectl get configmaps
- kubectl describe configmap
- kubectl get deploy
- kubectl get pods
- kubectl exec -- env
- ConfigMap update
- Deployment restart

---

## ✅ Summary

In this practical, I created a ConfigMap to store application configuration separately from the Docker image. The ConfigMap was injected into a Deployment as environment variables, verified inside the running container, and updated to demonstrate that environment variable changes require a Pod restart. This practical highlights the importance of separating configuration from application code and follows the DevOps principle of **Build Once, Deploy Everywhere**.
