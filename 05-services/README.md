# Services

## 📖 Introduction

A Service provides a stable network endpoint to access Pods.

---

## 🎯 Why Services?

Pods are ephemeral. When a Pod is recreated, it gets a new IP address.

A Service provides a stable IP address and DNS name to access Pods.

---

## Types of Services

### ClusterIP

- Default Service type
- Accessible only inside the cluster

### NodePort

- Exposes the application outside the cluster
- Uses a port in the range 30000–32767

### LoadBalancer

- Creates an external Load Balancer
- Used mainly in cloud environments

---

## Traffic Flow

```text
Internet
    │
LoadBalancer
    │
NodePort
    │
ClusterIP
    │
Pods
```

---

## Commands Used

```bash
kubectl expose deployment nginx-deployment --type=ClusterIP --port=80

kubectl expose deployment nginx-deployment --type=NodePort --port=80

kubectl expose deployment nginx-deployment --type=LoadBalancer --port=80

kubectl get svc

kubectl describe svc nginx-deployment

kubectl get endpoints

minikube service nginx-deployment --url

minikube tunnel
```

---

## Interview Questions

### Why do we need Services?

Because Pod IP addresses are not permanent.

### What is ClusterIP?

Default internal Service.

### What is NodePort?

Exposes the application outside the cluster.

### Why doesn't LoadBalancer work directly in Minikube?

Minikube doesn't have a cloud provider, so we use `minikube tunnel`.

---

## Summary

Services provide stable networking for Pods and allow applications to communicate reliably inside and outside the cluster.
