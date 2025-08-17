The **Sysdig Agent** is a lightweight software agent that runs on your host (VM, bare-metal, or Kubernetes/OpenShift node) to **collect system-level and container-level metrics, events, and logs**.

It is the core data collector for the **Sysdig platform** (used for monitoring, troubleshooting, security, and compliance).

---

### 🔹 What it does:

* **Captures system calls** (using Linux kernel instrumentation like `eBPF` or kernel modules).
* **Collects metrics**: CPU, memory, disk I/O, network, Kubernetes pod status, container health, etc.
* **Captures security events**: Unauthorized access, suspicious commands, file modifications.
* **Streams data** securely to the **Sysdig backend** (either Sysdig SaaS or an on-premises Sysdig instance).

---

### 🔹 Where it runs:

* **On each node** of your environment (VMs, Kubernetes/OpenShift worker/master nodes).
* It runs as a **DaemonSet** in Kubernetes/OpenShift (so one agent pod per node).

---

### 🔹 Why it’s used:

1. **Monitoring** – container and application performance metrics.
2. **Troubleshooting** – capture detailed system call activity.
3. **Security** – detect threats, compliance violations, and runtime anomalies.
4. **Forensics** – replay events that happened inside containers or the OS.

---

👉 In short:
**Sysdig Agent is the data collection engine** that feeds observability and security insights to the Sysdig platform.

Would you like me to show you how it looks when deployed in **OpenShift (as a DaemonSet YAML)** so you can connect it to Sysdig SaaS?
