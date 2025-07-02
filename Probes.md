In **OpenShift**, **probes** are used to **monitor the health** of your application containers. They help OpenShift decide:

* When a container is **ready to receive traffic**
* When a container is **unhealthy** and needs to be restarted

There are **three types of probes**:

---

### 1. **Liveness Probe** 🔁

* **Purpose**: Checks **if the container is still running** properly.
* **If it fails**: OpenShift **restarts** the container.

**Example use case**: App is stuck in a loop or deadlock — liveness probe catches it.

---

### 2. **Readiness Probe** 🟢

* **Purpose**: Checks if the container is **ready to serve requests**.
* **If it fails**: Container stays running, but OpenShift **removes it from the service endpoint** (no traffic routed to it).

**Example use case**: App is booting up or doing some warm-up task — probe ensures traffic is sent **only when ready**.

---

### 3. **Startup Probe** 🚦

* **Purpose**: Checks if the application has started **successfully**.
* **If it fails**: Container is killed and restarted.

**Use case**: For apps that take a long time to start (e.g., big Java apps), this prevents liveness/readiness probes from killing it too early.

---

### 🔧 How Probes Work

All three probe types can be defined using:

* **HTTP GET**: Sends an HTTP request to a specific endpoint (e.g., `/health`)
* **TCP Socket**: Checks if a TCP port is open
* **Exec**: Runs a command inside the container and checks the exit code

---

### 🧪 Example (in YAML)

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5

startupProbe:
  httpGet:
    path: /start
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

---

### 🧠 Real-time Scenario

Suppose your Java application takes 40 seconds to start. You can:

* Use **startupProbe** with `failureThreshold: 30` and `periodSeconds: 2` → gives it 60 seconds to start
* Once it passes, **liveness and readiness** probes take over


