**Tigera Calico** (commonly known as **Project Calico**) is an **open-source networking and network security solution** designed for **Kubernetes**, **OpenShift**, and other container or cloud-native environments.

---

### 🔍 What is Calico?

**Calico** provides:

1. **Container Networking** – Allows pods/containers to talk to each other across nodes.
2. **Network Security Policies** – Enforces rules on which pods/services can communicate.
3. **IP Address Management** – Assigns IP addresses to pods, uses BGP or IP-in-IP for routing.
4. **High Performance** – Uses **Linux kernel features (iptables, eBPF)** for fast data paths.

---

### 🧰 Key Features:

| Feature            | Description                                                                       |
| ------------------ | --------------------------------------------------------------------------------- |
| **CNI Plugin**     | Acts as a **Container Network Interface** to connect pods                         |
| **Network Policy** | Implements **Kubernetes Network Policies** or **Calico-specific policies**        |
| **IP Routing**     | Uses **BGP** or **VXLAN/IPIP** tunneling for pod-to-pod communication             |
| **Security**       | Supports **TLS encryption**, **firewalls**, and **Zero Trust network** principles |
| **Observability**  | Integration with **Tigera UI**, **Prometheus**, **Elasticsearch** for monitoring  |

---

### 🛡️ What is Tigera?

* **Tigera** is the company that **maintains and commercially supports Calico**.
* Offers **Calico Enterprise** – Adds advanced security, observability, and compliance features.

---

### 📦 Use Cases:

* **Kubernetes Networking**: Ensures pods can reach each other.
* **Microservice Security**: Restricts communication using **network policies**.
* **Hybrid Cloud**: Connects Kubernetes workloads across on-prem, AWS, Azure, GCP, etc.
* **Compliance**: Helps meet requirements (like PCI, HIPAA) through logging and controls.

---

### 📌 Example: Network Policy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
spec:
  podSelector:
    matchLabels:
      role: frontend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: backend
```

This policy allows only **backend pods** to talk to **frontend pods**.

---

Let me know if you want a diagram or flow to explain how Calico routes traffic inside a Kubernetes cluster.
