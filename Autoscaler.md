
### 🔹 **Cluster Autoscaler**

A **Cluster Autoscaler** automatically increases or decreases the number of **nodes** in the cluster based on workload demand. It works in conjunction with `MachineSets`.

* If pods are pending due to insufficient resources, it scales **up** (adds nodes).
* If nodes are underutilized, it scales **down** (removes nodes).

> It doesn’t directly manage pods or deployments — it just ensures enough nodes are available.

#### ✅ Example:

Suppose you deploy a new app that needs 5 CPUs, but your current nodes are full. The autoscaler will notice unscheduled pods and scale up by adding a new node via the associated `MachineSet`.

#### Sample ClusterAutoscaler YAML:

```yaml
apiVersion: autoscaling.openshift.io/v1
kind: ClusterAutoscaler
metadata:
  name: default
spec:
  scaleDown:
    enabled: true
    delayAfterAdd: 10m
    delayAfterDelete: 5m
  resourceLimits:
    maxNodesTotal: 10
```

---

### 🔸 Summary

| Component             | Purpose                        | Example                               |
| --------------------- | ------------------------------ | ------------------------------------- |
| **ClusterAutoscaler** | Dynamically adjusts node count | Adds/removes nodes based on pod needs |

Let me know if you want a diagram or YAML to deploy autoscaler in your OpenShift cluster.



| **ClusterAutoscaler** | Dynamically adjusts node count | Adds/removes nodes based on pod needs |


