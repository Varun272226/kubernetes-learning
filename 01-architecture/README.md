# Kubernetes Architecture

## 📖 Introduction

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications.

A Kubernetes cluster consists of a Control Plane and one or more Worker Nodes.

---

## 🎯 Why Kubernetes?

- Automates application deployment
- Automatically scales applications
- Self-healing by replacing failed Pods
- Load balancing
- Rolling updates and rollbacks
- Efficient resource utilization

---

## 🏗️ Architecture

```
                 Kubernetes Cluster
                        │
        ┌───────────────┴───────────────┐
        │                               │
 Control Plane                    Worker Node(s)
        │                               │
 ┌──────────────┐               ┌──────────────┐
 │ API Server   │               │ Kubelet      │
 │ Scheduler    │               │ Pods         │
 │ etcd         │               │ Containers   │
 └──────────────┘               └──────────────┘
```

---

## 🔑 Main Components

### API Server

- Entry point of Kubernetes
- Receives all kubectl requests
- Communicates with all cluster components

### etcd

- Key-value database
- Stores the complete cluster state

### Scheduler

- Selects the best Worker Node for new Pods

### Kubelet

- Runs on every Worker Node
- Creates and monitors Pods
- Reports node status to the API Server

---

## 💻 Commands Used

```bash
kubectl get nodes
kubectl cluster-info
kubectl version
```

---

## 🎤 Interview Questions

### What is Kubernetes?

Kubernetes is a container orchestration platform used to deploy, manage, and scale containerized applications.

### What is the Control Plane?

The Control Plane manages the entire Kubernetes cluster.

### What is etcd?

etcd is the database that stores the cluster state.

### What is the API Server?

The API Server is the entry point for all Kubernetes operations.

---

## ✅ Summary

The Control Plane manages the cluster, while Worker Nodes run the applications.
