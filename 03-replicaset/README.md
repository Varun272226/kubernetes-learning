# ReplicaSet

## 📖 Introduction

A ReplicaSet ensures that the desired number of Pod replicas are always running.

---

## 🎯 Why ReplicaSet?

- High Availability
- Self Healing
- Automatic Pod recreation
- Maintains desired number of Pods

---

## How ReplicaSet Works

ReplicaSet continuously watches the Pods.

If a Pod is deleted,

ReplicaSet automatically creates a new Pod.

---

## Commands Used

```bash
kubectl apply -f replicaset.yaml

kubectl get rs

kubectl describe rs nginx-rs

kubectl get pods

kubectl delete pod <pod-name>
```

---

## YAML Explanation

- apiVersion
- kind
- metadata
- spec
- replicas
- selector
- template

---

## Interview Questions

### What is ReplicaSet?

ReplicaSet maintains the desired number of Pods.

### What happens if a Pod is deleted?

ReplicaSet creates a new Pod automatically.

### Does ReplicaSet support Rolling Updates?

No.

Deployment supports Rolling Updates.

---

## Summary

ReplicaSet provides self-healing and high availability.
