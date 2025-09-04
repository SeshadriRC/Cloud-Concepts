Good question 👍

### **Job**

* **Purpose:** Runs a task **once** (or a fixed number of times until successful).
* **Use case:** Database migration, batch data processing, backup script.
* **Execution:** Starts pods, ensures they complete successfully. When done, the Job is complete.
* **Example:** Run `sleep 5` one time.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: one-time-job
spec:
  template:
    spec:
      containers:
        - name: job-container
          image: ubuntu:latest
          command: ["echo", "Hello from Job"]
      restartPolicy: Never
```

### **CronJob**

* **Purpose:** Runs a Job on a **schedule** (like Linux cron).
* **Use case:** Nightly backups, periodic cleanup, report generation.
* **Execution:** Creates Jobs at specified times (e.g., every day at midnight). Each Job then runs pods until completion.
* **Example:** Run `sleep 5` every day at 2 AM.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: scheduled-job
spec:
  schedule: "*/5 * * * *"   # every 5 minutes (cron syntax)
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: cronjob-container
              image: ubuntu:latest
              command: ["echo", "Hello from CronJob"]
          restartPolicy: Never
```

📌 In short:

* **Job** → one-time or limited runs.
* **CronJob** → scheduled, recurring jobs (uses Job under the hood).

