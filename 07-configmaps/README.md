# Kubernetes ConfigMaps

---

## 📖 Introduction

A ConfigMap is a Kubernetes API object used to store non-sensitive configuration as key-value pairs. It separates configuration from the application code and Docker image, allowing the same container image to be deployed across multiple environments with different configurations.

ConfigMaps can be consumed in two ways:

- Environment Variables
- Mounted Volumes

---

## 🎯 Objectives

- Understand the purpose of ConfigMaps.
- Store application configuration separately from the Docker image.
- Use ConfigMaps as environment variables.
- Use ConfigMaps as mounted volumes.
- Verify ConfigMap values inside a running container.
- Understand how ConfigMap updates behave in different scenarios.

---

## ❓ Why do we need ConfigMaps?

Applications require configuration such as:

- Database host
- Application environment
- Log level
- Feature flags

Hardcoding these values inside the application means every configuration change requires rebuilding and redeploying the Docker image.

ConfigMaps solve this problem by separating configuration from the application.

This follows the DevOps principle:

> **Build Once, Deploy Everywhere**

The same Docker image can be deployed in Development, Testing, and Production while using different ConfigMaps.

---

## 🏗️ Architecture

### ConfigMap as Environment Variables

```text
ConfigMap
     │
     ▼
Deployment
     │
     ▼
Pod
     │
     ▼
Environment Variables
```

### ConfigMap as Volume

```text
ConfigMap
     │
     ▼
Volume
     │
     ▼
Deployment
     │
     ▼
Pod
     │
     ▼
/etc/config
     │
 ┌───────────────┐
 │ APP_ENV       │
 │ DATABASE_HOST │
 │ LOG_LEVEL     │
 └───────────────┘
```

---

## 📂 Files

```text
configmap.yaml
deployment.yaml
configmap-volume.yaml
deployment-volume.yaml
README.md
```

---

## 💻 Commands Used

### Create ConfigMaps

```bash
kubectl apply -f configmap.yaml

kubectl apply -f configmap-volume.yaml
```

### Verify ConfigMaps

```bash
kubectl get configmaps

kubectl describe configmap app-config

kubectl describe configmap app-config-volume
```

### Deploy Applications

```bash
kubectl apply -f deployment.yaml

kubectl apply -f deployment-volume.yaml
```

### Verify Deployments

```bash
kubectl get deploy

kubectl get pods
```

### Verify Environment Variables

```bash
kubectl exec -it <pod-name> -- env

kubectl exec -it <pod-name> -- env | grep APP

kubectl exec -it <pod-name> -- env | grep DATABASE

kubectl exec -it <pod-name> -- env | grep LOG
```

### Verify Mounted Files

```bash
kubectl exec -it <pod-name> -- ls /etc/config

kubectl exec -it <pod-name> -- cat /etc/config/APP_ENV

kubectl exec -it <pod-name> -- cat /etc/config/DATABASE_HOST

kubectl exec -it <pod-name> -- cat /etc/config/LOG_LEVEL
```

### Restart Deployment

```bash
kubectl rollout restart deployment configmap-demo
```

---

## 🧠 Concepts Learned

- ConfigMaps store non-sensitive configuration.
- Configuration should be separated from the application.
- ConfigMaps can be consumed as environment variables.
- ConfigMaps can also be mounted as volumes.
- One ConfigMap key becomes one file when mounted as a volume.
- Environment variable updates require Pod restart.
- Mounted ConfigMap files are updated by Kubernetes; whether the application uses the new values immediately depends on whether it reloads its configuration.

---

## 🎤 Interview Questions

### What is a ConfigMap?

A ConfigMap stores non-sensitive configuration as key-value pairs and separates configuration from the application.

---

### What are the two ways to use a ConfigMap?

- Environment Variables
- Mounted Volumes

---

### What happens if a ConfigMap is updated?

**Environment Variables**

- Running Pods continue using the old values.
- Pod restart is required.

**Mounted Volumes**

- Kubernetes updates the mounted files.
- Some applications automatically reload the updated files, while others require a reload or restart.

---

### Why shouldn't passwords be stored in ConfigMaps?

ConfigMaps store non-sensitive configuration.

Passwords, API keys, tokens, and certificates should be stored in Kubernetes Secrets.

---

### If a ConfigMap contains 100 keys and is mounted as a volume, how many files are created?

100 files.

Each key becomes one file.

---

## ⚠️ Troubleshooting

### Pod does not start

Verify the ConfigMap exists.

```bash
kubectl get configmaps
```

---

### ConfigMap key not found

Ensure the key referenced in the Deployment matches the key in the ConfigMap.

---

### Updated ConfigMap but application still shows old values

If using environment variables:

```bash
kubectl rollout restart deployment configmap-demo
```

If using mounted volumes, verify whether the application supports dynamic configuration reloads.

---

## 📸 Screenshots

Screenshots will be added later.

- ConfigMap creation
- ConfigMap description
- Deployment creation
- Environment variables
- Mounted ConfigMap files
- ConfigMap update
- Deployment restart

---

## ✅ Summary

In this practical, I learned how to use Kubernetes ConfigMaps to manage non-sensitive configuration separately from the application image. I used ConfigMaps as both environment variables and mounted volumes, verified the configuration inside running containers, and understood the different behaviors when updating ConfigMaps. This demonstrates how Kubernetes enables reusable container images while keeping configuration flexible across environments.



