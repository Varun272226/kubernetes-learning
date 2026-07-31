# Kubernetes DaemonSet

## Objective

Learn how to use **DaemonSets** in Kubernetes to ensure that a Pod runs on every eligible node in a cluster.

---

## What is a DaemonSet?

A **DaemonSet** is a Kubernetes workload resource that ensures **one Pod runs on every eligible node** in the cluster.

Unlike a Deployment, you do not specify the number of replicas. Kubernetes automatically creates and manages one Pod per node.

---

## Why Do We Need a DaemonSet?

Some applications must run on every node instead of a fixed number of replicas.

Examples include:

- Log collection agents (Fluentd, Fluent Bit)
- Monitoring agents (Prometheus Node Exporter)
- Network plugins (Calico, Cilium)
- Security agents (Falco)

DaemonSets ensure these applications are available on every node automatically.

---

## How DaemonSet Works

Suppose your cluster has three worker nodes.

```
Node-1
Node-2
Node-3
```

After creating a DaemonSet:

```
Node-1 → Daemon Pod
Node-2 → Daemon Pod
Node-3 → Daemon Pod
```

If a new node joins the cluster:

```
Node-4
```

Kubernetes automatically creates another DaemonSet Pod on Node-4.

If a node is removed, the corresponding DaemonSet Pod is removed automatically.

---

## Deployment vs DaemonSet

| Deployment | DaemonSet |
|------------|-----------|
| Fixed number of replicas | One Pod per eligible node |
| User specifies replicas | Kubernetes determines the number of Pods |
| Used for web applications | Used for node-level services |

---

## Practical

### File

```
daemonset.yaml
```

Creates an Nginx DaemonSet.

Each eligible node in the cluster runs one Nginx Pod.

---

## YAML Explanation

### apiVersion

```yaml
apiVersion: apps/v1
```

Specifies the API version.

---

### kind

```yaml
kind: DaemonSet
```

Creates a DaemonSet resource.

---

### metadata

```yaml
metadata:
  name: nginx-daemon
```

Assigns the name of the DaemonSet.

---

### selector

```yaml
selector:
  matchLabels:
    app: nginx-daemon
```

Identifies which Pods belong to the DaemonSet.

---

### template

Defines the Pod template that Kubernetes creates on each node.

---

### containers

Runs an Nginx container.

```yaml
containers:
- name: nginx
  image: nginx
```

---

## Commands Used

### Create DaemonSet

```bash
kubectl apply -f daemonset.yaml
```

---

### View DaemonSets

```bash
kubectl get daemonsets
```

or

```bash
kubectl get ds
```

---

### View Pods

```bash
kubectl get pods -o wide
```

The `NODE` column shows which node each Pod is running on.

---

### Describe DaemonSet

```bash
kubectl describe daemonset nginx-daemon
```

---

### Delete DaemonSet

```bash
kubectl delete daemonset nginx-daemon
```

---

## Expected Output

For a single-node Minikube cluster:

```
NAME            DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE
nginx-daemon    1         1         1       1            1
```

For a three-node cluster:

```
NAME            DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE
nginx-daemon    3         3         3       3            3
```

---

## Verify the Node

Run:

```bash
kubectl get pods -o wide
```

Example:

```
NAME                  READY   STATUS    NODE
nginx-daemon-abc12    1/1     Running   minikube
```

---

## Interview Questions

### 1. What is a DaemonSet?

A DaemonSet ensures that one Pod runs on every eligible node in a Kubernetes cluster.

---

### 2. How is a DaemonSet different from a Deployment?

A Deployment runs a specified number of replicas, while a DaemonSet automatically runs one Pod on every eligible node.

---

### 3. Give examples of applications that use DaemonSets.

- Fluentd
- Fluent Bit
- Prometheus Node Exporter
- Calico
- Cilium
- Falco

---

### 4. What happens when a new node is added?

Kubernetes automatically schedules a new DaemonSet Pod on the new node.

---

### 5. Does a DaemonSet require a `replicas` field?

No. Kubernetes automatically creates one Pod for each eligible node.

---

## Folder Structure

```
16-daemonset/
├── daemonset.yaml
├── README.md
└── images/
```

---

## Key Takeaways

- A DaemonSet runs one Pod on every eligible node.
- It is commonly used for logging, monitoring, networking, and security agents.
- New nodes automatically receive a DaemonSet Pod.
- DaemonSets do not use a `replicas` field.
- They are ideal for node-level services.
