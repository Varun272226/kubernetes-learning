# Kubernetes Jobs

## Objective

Learn how Kubernetes Jobs execute one-time tasks that run until completion. Jobs are commonly used for batch processing, database migrations, backups, and report generation.

---

## What is a Job?

A **Job** is a Kubernetes workload resource that creates one or more Pods and ensures they successfully complete a task.

Unlike a Deployment, a Job does not keep Pods running continuously.

Once the task completes successfully, the Job is marked as **Completed**.

---

## Why Do We Need Jobs?

Some tasks should run only once instead of continuously.

Examples:

- Database migration
- Database backup
- Report generation
- Batch processing
- Data import/export
- Initial application setup

---

## Deployment vs Job

| Deployment | Job |
|------------|-----|
| Runs continuously | Runs until completion |
| Maintains desired replicas | Stops after successful execution |
| Used for web applications | Used for one-time tasks |

---

## Practical 1

### File

```
job.yaml
```

Creates a Job that prints:

```
Hello Kubernetes
```

and exits successfully.

---

## Practical 2

### File

```
failed-job.yaml
```

Creates a Job that intentionally fails by executing:

```bash
exit 1
```

Kubernetes retries the Job until the configured `backoffLimit` is reached.

---

## YAML Explanation

### apiVersion

```yaml
apiVersion: batch/v1
```

Jobs belong to the **batch** API group.

---

### kind

```yaml
kind: Job
```

Creates a Kubernetes Job.

---

### metadata

```yaml
metadata:
  name: hello-job
```

Name of the Job.

---

### template

Defines the Pod that the Job creates.

---

### containers

```yaml
containers:
- name: hello
  image: busybox
```

Uses the lightweight BusyBox container.

---

### command

```yaml
command:
- sh
- -c
- echo "Hello Kubernetes"
```

Executes a shell command inside the container.

---

### restartPolicy

```yaml
restartPolicy: Never
```

Do not restart the container after it exits.

Jobs manage retries themselves instead of relying on Pod restart policies.

---

### backoffLimit

```yaml
backoffLimit: 3
```

Maximum number of retry attempts before the Job is marked as failed.

---

## Commands

### Create Job

```bash
kubectl apply -f job.yaml
```

---

### Create Failed Job

```bash
kubectl apply -f failed-job.yaml
```

---

### View Jobs

```bash
kubectl get jobs
```

---

### View Pods

```bash
kubectl get pods
```

---

### Describe Job

```bash
kubectl describe job hello-job
```

---

### View Logs

```bash
kubectl logs job/hello-job
```

or

```bash
kubectl logs <pod-name>
```

---

### Delete Job

```bash
kubectl delete job hello-job
kubectl delete job failed-job
```

---

## Expected Output

Successful Job:

```
NAME        COMPLETIONS   DURATION   AGE
hello-job   1/1           3s         10s
```

Failed Job:

```
NAME         COMPLETIONS   DURATION   AGE
failed-job   0/1           25s        30s
```

---

## Interview Questions

### 1. What is a Kubernetes Job?

A Job runs a task until it completes successfully.

---

### 2. How is a Job different from a Deployment?

A Deployment keeps Pods running continuously, while a Job finishes after completing its task.

---

### 3. What is `restartPolicy: Never`?

It tells Kubernetes not to restart the container after it exits. The Job controller manages retries.

---

### 4. What is `backoffLimit`?

The maximum number of retries before Kubernetes marks the Job as failed.

---

### 5. Give some real-world use cases for Jobs.

- Database migration
- Database backup
- Batch processing
- Report generation
- Data import/export

---

## Folder Structure

```
18-jobs/
├── job.yaml
├── failed-job.yaml
├── README.md
└── images/
```

---

## Key Takeaways

- Jobs execute one-time tasks.
- Jobs stop after successful completion.
- `backoffLimit` controls retry attempts.
- `restartPolicy: Never` prevents Pod restarts.
- Jobs are ideal for batch workloads and maintenance tasks.
