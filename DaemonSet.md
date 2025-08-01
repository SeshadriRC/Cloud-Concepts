A **DaemonSet** in OpenShift (or Kubernetes) is a special type of workload that ensures **a copy of a pod runs on every (or selected) node** in the cluster.

---

### 📌 Purpose of DaemonSet:

To run **one pod per node**, usually for **system-level or background services** like:

* **Log collection** (e.g., Fluentd, Logstash)
* **Monitoring agents** (e.g., Datadog, Prometheus Node Exporter)
* **Storage plugins** (e.g., CSI drivers)
* **Security agents** (e.g., antivirus, audit daemons)

---

### 🔧 Example use case:

If you want to collect logs from all nodes, you'd create a DaemonSet so each node gets a pod that ships logs to a central server.

---

### 🧱 Behavior:

* When a new node is added, a DaemonSet **automatically creates a pod** on it.
* When a node is removed, the pod is removed too.
* You can configure it to run on **specific nodes only**, using `nodeSelector`, `affinity`, or `tolerations`.

---

### 💡 Example YAML:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-logger
spec:
  selector:
    matchLabels:
      name: node-logger
  template:
    metadata:
      labels:
        name: node-logger
    spec:
      containers:
      - name: logger
        image: fluentd:latest
```

This would run a Fluentd pod on **every node**.

---

Let me know if you want an example specific to OpenShift or want to view running DaemonSets in your cluster.
