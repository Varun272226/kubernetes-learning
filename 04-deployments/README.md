# Deployments

## 📖 Introduction

A Deployment manages ReplicaSets and Pods.

It provides rolling updates, rollbacks, and scaling.

---

## 🎯 Why Deployments?

- Rolling Updates
- Rollbacks
- Scaling
- High Availability
- Declarative application management

---

## Architecture

```text
Deployment
     │
ReplicaSet
     │
Pods
```

---

## Commands Used

```bash
kubectl apply -f deployment.yaml

kubectl get deploy

kubectl describe deployment nginx-deployment

kubectl edit deployment nginx-deployment

kubectl rollout status deployment nginx-deployment

kubectl rollout history deployment nginx-deployment

kubectl rollout undo deployment nginx-deployment

kubectl scale deployment nginx-deployment --replicas=5
```

---

## Interview Questions

### What is a Deployment?

Deployment manages ReplicaSets and Pods.

### Difference between Deployment and ReplicaSet?

ReplicaSet manages Pods.

Deployment manages ReplicaSets.

### Does Deployment support rollback?

Yes.

---

## Summary

Deployment is the recommended way to deploy applications in Kubernetes.
