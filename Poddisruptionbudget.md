A **PodDisruptionBudget (PDB)** in Kubernetes (including OpenShift) is a **policy that limits the number of pods of a replicated application that can be voluntarily disrupted** at a time (e.g., during node drain, upgrade, etc.).

---

### ✅ **Why PodDisruptionBudget is Used**

To ensure **high availability** of your application during:

* Node maintenance (drain/cordon)
* Cluster upgrades
* Voluntary disruptions

It **does not prevent involuntary disruptions** like node failures or crashes.

---

### 🧩 Key Components / Attributes

| Attribute                    | Description                                                |
| ---------------------------- | ---------------------------------------------------------- |
| `minAvailable`               | Minimum number of pods that must be available at any time. |
| `maxUnavailable`             | Maximum number of pods that can be unavailable at a time.  |
| `selector`                   | Label selector to specify which pods the PDB applies to.   |
| `unhealthyPodEvictionPolicy` | (Optional) Controls eviction behavior for unhealthy pods.  |

You must specify **either `minAvailable` or `maxUnavailable`, not both**.

---

### 📦 Example PodDisruptionBudget YAML

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: myapp-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: myapp
```

✅ **Explanation**:

* This ensures at least **2 pods of `myapp` must be running** at all times.
* If only 2 pods are running and you try to drain a node with 1 of them, it will **block the drain**.

---

### 🧠 Example Scenario

Let’s say your Deployment has 3 replicas of a pod:

| Pods Running | `minAvailable: 2` | Can Disrupt One Pod? |
| ------------ | ----------------- | -------------------- |
| 3            | ✅                 | Yes                  |
| 2            | ✅                 | No                   |
| 1            | ❌                 | No                   |

---

### 🔒 Best Practices

* Set PDBs for all mission-critical workloads.
* Don’t set `minAvailable` to 100% of replicas unless you **never want disruptions**.
* Use with **Deployments, StatefulSets, ReplicaSets**, etc.


No problem! Let's simplify the **example scenario** with a visual-style explanation.

---

### 🧪 Scenario

You have a **Deployment** with **3 pods**:

```
myapp-pod-1   ✅ Running
myapp-pod-2   ✅ Running
myapp-pod-3   ✅ Running
```

And you define a **PodDisruptionBudget** like this:

```yaml
minAvailable: 2
```

This means:
👉 **At least 2 pods must be running at all times.**

---

### 🤔 Now What Happens During a Node Drain?

Let's say a node running one of the pods is being drained (e.g., for maintenance). Kubernetes will **evict the pod** on that node.

Let’s evaluate what happens:

---

#### ✅ **Case 1** – 3 Pods Running

```
Running Pods: 3
minAvailable: 2
Evicting 1 pod? ✅ Allowed
```

✅ It's okay to **evict 1 pod**, because after eviction, **2 will still be running**, which satisfies `minAvailable: 2`.

---

#### ❌ **Case 2** – 2 Pods Running

```
Running Pods: 2
minAvailable: 2
Evicting 1 pod? ❌ Not Allowed
```

❌ **Eviction is blocked**, because if 1 pod is evicted, only 1 will remain, which **violates** the policy.

---

#### ❌ **Case 3** – 1 Pod Running

```
Running Pods: 1
minAvailable: 2
Evicting 1 pod? ❌ Not Allowed
```

❌ Same logic – you can’t go below 2 running pods.

---

### 🔁 Summary Table

| Pods Running | Can Evict 1 Pod? | Reason                         |
| ------------ | ---------------- | ------------------------------ |
| 3            | ✅ Yes            | 2 still running after eviction |
| 2            | ❌ No             | Would leave only 1 running     |
| 1            | ❌ No             | Would leave 0 running          |

---

Let me know if you'd like a real OpenShift PDB example using `oc create pdb` or want to test this yourself.

---

Let me know if you want an OpenShift-specific example using `oc` CLI.


