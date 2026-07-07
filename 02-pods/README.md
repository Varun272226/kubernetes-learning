# Pods

## 📖 Introduction

A Pod is the smallest deployable unit in Kubernetes.

A Pod contains one or more containers that share the same network and storage.

---

## 🎯 Why Pods?

- Runs application containers
- Provides shared networking
- Provides shared storage
- Basic execution unit in Kubernetes

---

## Pod Lifecycle

```
Pending
   │
Running
   │
Succeeded / Failed
```

---

## Commands Used

```bash
kubectl run nginx --image=nginx

kubectl get pods

kubectl describe pod nginx

kubectl delete pod nginx
```

---

## YAML Used

nginx-pod.yaml

apache-pod.yaml

---

## Interview Questions

### What is a Pod?

A Pod is the smallest deployable unit in Kubernetes.

### Can a Pod contain multiple containers?

Yes.

### Do containers inside the same Pod share networking?

Yes.

---

## Summary

Pods are the basic execution units of Kubernetes and contain one or more containers.
