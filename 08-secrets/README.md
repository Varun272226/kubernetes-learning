# Kubernetes Secrets

---

## 📖 Introduction

A Secret is a Kubernetes API object used to store sensitive information such as passwords, API keys, access tokens, and certificates. Secrets allow confidential data to be managed separately from application code and Docker images.

Unlike ConfigMaps, Secrets are specifically designed for storing sensitive configuration.

---

## 🎯 Objectives

- Understand why Kubernetes Secrets are required.
- Learn the difference between ConfigMaps and Secrets.
- Create a Secret using YAML.
- Inject Secret values into a Pod as environment variables.
- Verify Secret values inside a running container.
- Understand the difference between `data` and `stringData`.

---

## ❓ Why do we need Secrets?

Applications often require sensitive information such as:

- Database username
- Database password
- API keys
- Access tokens
- Certificates

Storing this information in ConfigMaps or hardcoding it inside an application is not recommended.

Kubernetes Secrets provide a dedicated resource for storing sensitive data separately from the application.

---

## 🏗️ Architecture

```text
                Secret
                   │
                   │
     DATABASE_USERNAME
     DATABASE_PASSWORD
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

---

## 📂 Files

```text
secret.yaml
deployment.yaml
README.md
```

---

## 💻 Commands Used

### Create the Secret

```bash
kubectl apply -f secret.yaml
```

Verify the Secret

```bash
kubectl get secrets

kubectl describe secret db-secret
```

Deploy the application

```bash
kubectl apply -f deployment.yaml
```

Verify the Deployment

```bash
kubectl get deploy

kubectl get pods
```

Verify Secret values inside the Pod

```bash
kubectl exec -it <pod-name> -- env

kubectl exec -it <pod-name> -- env | grep DATABASE
```

View Secret YAML

```bash
kubectl get secret db-secret -o yaml
```

---

## 🔄 Request Flow

```text
Secret

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

- Secrets store sensitive information.
- Secrets separate confidential data from application code.
- Secrets can be injected into Pods as environment variables.
- Kubernetes supports both `data` and `stringData`.
- `stringData` accepts plain text.
- Kubernetes automatically converts `stringData` into Base64 encoded `data`.
- Base64 encoding is **not** encryption.
- Secret values are hidden when using `kubectl describe secret`.

---

## 🎤 Interview Questions

### What is a Secret?

A Secret is a Kubernetes resource used to store sensitive information such as passwords, API keys, tokens, and certificates.

---

### Why should passwords not be stored in ConfigMaps?

ConfigMaps are intended for non-sensitive configuration. Sensitive information should be stored in Secrets.

---

### What is the difference between `data` and `stringData`?

**data**

- Values must already be Base64 encoded.

**stringData**

- Accepts plain text.
- Kubernetes automatically converts the values to Base64 when the Secret is created.

---

### Is Base64 encryption?

No.

Base64 is an encoding format, not an encryption mechanism.

---

### Does `kubectl describe secret` display Secret values?

No.

It only displays the keys and the size of the stored values to help avoid accidental exposure.

---

## ⚠️ Troubleshooting

### Pod does not start

Verify that the Secret exists.

```bash
kubectl get secrets
```

---

### Secret key not found

Ensure the key referenced in the Deployment matches the key defined in the Secret.

---

### Verify Secret values inside the Pod

```bash
kubectl exec -it <pod-name> -- env | grep DATABASE
```

---

## 📸 Screenshots

Screenshots will be added later.

- Secret creation
- kubectl get secrets
- kubectl describe secret
- Deployment creation
- Pod creation
- Secret environment variables
- Secret YAML

---

## ✅ Summary

In this practical, I created a Kubernetes Secret to store sensitive configuration separately from the application. The Secret was injected into a Deployment as environment variables and verified inside the running container. I also learned the difference between `data` and `stringData`, understood that Base64 encoding is not encryption, and compared the purpose of Secrets with ConfigMaps.
