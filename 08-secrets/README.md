# Kubernetes Secrets

---

## 📖 Introduction

A Secret is a Kubernetes API object used to store sensitive information such as passwords, API keys, access tokens, and certificates. Secrets separate confidential information from application code and Docker images, improving security and making applications easier to manage.

Secrets can be consumed in two ways:

- Environment Variables
- Mounted Volumes

---

## 🎯 Objectives

- Understand why Kubernetes Secrets are required.
- Learn the difference between ConfigMaps and Secrets.
- Create Secrets using YAML.
- Inject Secrets into Pods as environment variables.
- Mount Secrets as volumes.
- Verify Secret values inside a running container.
- Understand the difference between `data` and `stringData`.

---

## ❓ Why do we need Secrets?

Applications require sensitive information such as:

- Database username
- Database password
- API keys
- Access tokens
- TLS certificates

Hardcoding these values inside application code or storing them in ConfigMaps is not recommended.

Kubernetes Secrets provide a dedicated resource for storing sensitive configuration separately from the application.

---

## 🏗️ Architecture

### Secret as Environment Variables

```text
                 Secret
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

### Secret as Mounted Volume

```text
                 Secret
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
               /etc/secret
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
 DATABASE_USERNAME   DATABASE_PASSWORD
```

---

## 📂 Files

```text
secret.yaml
deployment.yaml
secret-volume.yaml
deployment-volume.yaml
README.md
```

---

## 💻 Commands Used

### Create Secrets

```bash
kubectl apply -f secret.yaml

kubectl apply -f secret-volume.yaml
```

### Verify Secrets

```bash
kubectl get secrets

kubectl describe secret db-secret

kubectl describe secret db-secret-volume
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

kubectl exec -it <pod-name> -- env | grep DATABASE
```

### Verify Mounted Secret Files

```bash
kubectl exec -it <pod-name> -- ls /etc/secret

kubectl exec -it <pod-name> -- cat /etc/secret/DATABASE_USERNAME

kubectl exec -it <pod-name> -- cat /etc/secret/DATABASE_PASSWORD
```

### View Secret YAML

```bash
kubectl get secret db-secret -o yaml
```

---

## 🧠 Concepts Learned

- Secrets store sensitive information.
- Secrets separate confidential data from application code.
- Secrets can be consumed as environment variables.
- Secrets can also be mounted as volumes.
- One Secret key becomes one file when mounted as a volume.
- Environment variable updates require a Pod restart.
- Mounted Secret files are updated by Kubernetes. Whether the application uses the new values immediately depends on whether it reloads the files.
- `stringData` accepts plain text.
- Kubernetes automatically converts `stringData` into Base64 encoded `data`.
- Base64 encoding is not encryption.

---

## 🎤 Interview Questions

### What is a Secret?

A Secret is a Kubernetes resource used to store sensitive information such as passwords, API keys, access tokens, and certificates.

---

### Why should passwords not be stored in ConfigMaps?

ConfigMaps are intended for non-sensitive configuration. Sensitive information should be stored in Secrets.

---

### What is the difference between `data` and `stringData`?

**data**

- Values must already be Base64 encoded.

**stringData**

- Accepts plain text.
- Kubernetes automatically converts the values into Base64 encoded `data`.

---

### Is Base64 encryption?

No.

Base64 is an encoding format, not an encryption mechanism.

---

### What happens when a Secret is updated?

**Environment Variables**

- Running Pods continue using the old values.
- Restart the Pod or Deployment to load the updated Secret.

**Mounted Volumes**

- Kubernetes updates the mounted Secret files.
- Whether the application immediately uses the new values depends on whether it reloads the files.

---

### If a Secret contains 10 keys and is mounted as a volume, how many files are created?

10 files.

Each Secret key becomes one file.

---

## 🔄 ConfigMap vs Secret

| ConfigMap | Secret |
|------------|---------|
| Stores non-sensitive configuration | Stores sensitive information |
| Database host, log level, environment | Passwords, API keys, tokens, certificates |
| Plain text values | Base64 encoded values (stored internally) |
| Used for application configuration | Used for confidential data |

---

## ⚠️ Troubleshooting

### Pod does not start

Verify the Secret exists.

```bash
kubectl get secrets
```

---

### Secret key not found

Ensure the key referenced in the Deployment matches the key defined in the Secret.

---

### Secret updated but Pod still shows old values

If using environment variables:

```bash
kubectl rollout restart deployment secret-demo
```

If using mounted volumes, verify whether the application reloads the updated files.

---

## 📸 Screenshots

Screenshots will be added later.

- Secret creation
- kubectl get secrets
- kubectl describe secret
- Deployment creation
- Pod creation
- Environment variables
- Mounted Secret files
- Secret YAML
- Deployment restart

---

## ✅ Summary

In this practical, I created Kubernetes Secrets to securely store sensitive information and consumed them using both environment variables and mounted volumes. I verified Secret values inside a running container, learned the difference between `data` and `stringData`, understood that Base64 encoding is not encryption, and compared Secrets with ConfigMaps. This practical demonstrates how Kubernetes securely manages confidential application configuration while following production best practices.
