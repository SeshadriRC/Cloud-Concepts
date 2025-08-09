In OpenShift, **HPA** stands for **Horizontal Pod Autoscaler**.

It’s a Kubernetes/Openshift feature that **automatically scales the number of pod replicas** in a deployment, deploymentConfig, replica set, or replication controller based on observed CPU utilization (or other custom metrics).

---

### **How it works**

1. You define a target metric (e.g., CPU usage should stay at 50%).
2. HPA continuously checks metrics from the cluster (via the metrics server).
3. If the metric exceeds the target, it increases pod replicas; if it’s below, it decreases them.
4. Scaling happens between a **minimum** and **maximum** number of replicas you define.

---

<img width="1600" height="720" alt="image" src="https://github.com/user-attachments/assets/6ec76031-b737-4438-942e-2f9fa48fdcda" />


### **Basic Example**

```bash
oc autoscale dc/my-app --min=2 --max=10 --cpu-percent=50
```

* **dc/my-app** → DeploymentConfig to scale
* **--min=2** → Minimum 2 pods
* **--max=10** → Maximum 10 pods
* **--cpu-percent=50** → Scale to keep average CPU usage at \~50% per pod

---

### **Key Points**

* Works best with **stateless** applications.
* Needs **metrics server** (in OpenShift, “Cluster Metrics”) enabled.
* Supports **CPU** and **memory** usage, plus **custom metrics** via Prometheus Adapter.
* Scaling happens **horizontally** (adds/removes pods) — not vertically (changing pod CPU/RAM limits).

---


