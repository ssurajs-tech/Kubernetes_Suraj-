---

# 📘 Kubernetes Batch Jobs and CronJobs

This repository contains examples of **Kubernetes Job** and **CronJob** manifests.
Jobs and CronJobs are part of Kubernetes **batch workloads**, designed for tasks that run to completion (unlike Deployments which are long-running).

---

## 🔹 1. CronJob Example

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: trupti
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - image: ubuntu
            name: trupti
            command: ["/bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
          restartPolicy: Never
```

### 📝 Explanation

* **kind: CronJob** → Runs jobs on a repeating schedule (like Linux cron).
* **metadata.name: trupti** → Name of the CronJob.
* **schedule: "* \* \* \* *"** → Runs every minute.
* **jobTemplate** → Defines the Pod that will run when the schedule triggers.
* **container** → Runs an `ubuntu` container, executes a command:

  * Prints `"Technical-Guftgu"`
  * Sleeps for 5 seconds
* **restartPolicy: Never** → The Pod won’t restart automatically.

### 🚀 Result

This will **print "Technical-Guftgu" every minute** in the logs.

---

## 🔹 2. Parallel Job with Timeout

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: testpod
spec:
  parallelism: 5                           # Runs 5 Pods in parallel
  activeDeadlineSeconds: 10                # Job will timeout after 10 seconds
  template:
    metadata:
      name: testpod
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 30"]
      restartPolicy: Never
```

### 📝 Explanation

* **kind: Job** → Runs Pods until completion.
* **parallelism: 5** → Creates **5 Pods running in parallel**.
* **activeDeadlineSeconds: 10** → Kills the Job if it doesn’t finish within **10 seconds**.
* **container** → Runs a CentOS 7 container, executes:

  * Prints `"Technical-Guftgu"`
  * Sleeps for 30 seconds
* Since the **sleep (30s)** is longer than the **deadline (10s)**, the Job will fail.

### 🚀 Result

This example shows **Job timeout handling**. Even though Pods are running, they will be **terminated after 10 seconds**.

---

## 🔹 3. Simple Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  template:
    metadata:
      name: testpod
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
      restartPolicy: Never
```

### 📝 Explanation

* **kind: Job** → Runs a task to completion.
* **metadata.name: testjob** → Job name.
* **container** → Runs a CentOS 7 container, executes:

  * Prints `"Technical-Guftgu"`
  * Sleeps for 5 seconds.
* **restartPolicy: Never** → No restart after completion.

### 🚀 Result

This Job will run **a single Pod once**, complete successfully in 5 seconds, and then exit.

---

## ⚡ Usage Instructions

### 1. Apply the manifests

```sh
kubectl apply -f cronjob.yaml
kubectl apply -f parallel-job.yaml
kubectl apply -f simple-job.yaml
```

### 2. Check Jobs and CronJobs

```sh
kubectl get jobs
kubectl get cronjobs
kubectl get pods
```

### 3. View logs

```sh
kubectl logs <pod-name>
```

---

## ✅ Summary

* **CronJob** → Runs tasks on a schedule (like cron).
* **Job with parallelism & deadline** → Runs multiple Pods but demonstrates timeouts.
* **Simple Job** → Runs a single task to completion.

These examples are great for learning **batch workloads in Kubernetes**.

---

