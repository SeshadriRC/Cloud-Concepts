In OpenShift (and Kubernetes), probe failures occur when liveness, readiness, or startup probes defined in pod specs fail, often leading to pod restarts or traffic being cut off. Understanding and fixing these is crucial for keeping your applications healthy.

| Probe Type    | Purpose                                                    | Impact on Failure                |
| ------------- | ---------------------------------------------------------- | -------------------------------- |
| **Readiness** | Checks if the app is ready to receive traffic              | Pod removed from service routing |
| **Liveness**  | Checks if the app is still alive                           | Pod is **restarted**             |
| **Startup**   | Ensures app has started before liveness/readiness kicks in | Prevents premature restarts      |

🛠️ Common Causes and Fixes

**1. Incorrect Probe Configuration**
   
Issue: Wrong path, port, or timeout.

Fix: Validate the probe definition:
```
readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
  timeoutSeconds: 2
```
✅ Check:

Is the path correct?

Is the app listening on that port?

Is the endpoint responding fast enough?

**2. Application Not Ready in Time**
   
Issue: App takes longer to start than initialDelaySeconds.

Fix: Increase initialDelaySeconds or use a startupProbe:

3. Slow or Overloaded Application
   
Issue: High resource usage causes delayed probe responses.

Fix:

Increase CPU/memory limits or requests.

Tune timeoutSeconds or periodSeconds in probe.

Optimize app performance or remove unnecessary work in probe endpoint.

4. Network or Firewall Issues
   
Issue: Probes can't reach the container due to network restrictions.

Fix: Ensure the pod can respond to internal traffic and the containerPort is exposed properly.

5. Non-200 HTTP Responses
   
Issue: The probe endpoint returns 500 or other non-success codes.

Fix: Make sure the /health or /ready endpoints return 200 when healthy. Modify the app if needed.

📌 Example Fix

Problem:

Readiness probe failed: HTTP probe failed with statuscode: 500

Action:

Hit the endpoint manually: curl <pod-ip>:8080/health

Fix the application logic to return 200 on healthy status

Or update the probe to point to the correct, working path

✅ Best Practices

Always set reasonable delays (initialDelaySeconds) for slow-starting apps.

Use startupProbe for apps with long initialization times.

Monitor probe failures via oc describe pod <pod> and logs.

Avoid heavy work in health endpoints.

**1. Readiness Probe Failure**
   
❗ What happens:
Pod stays in Running state but is not added to the service endpoint list, so it receives no traffic.

🛠️ Common Causes & Fixes:

Cause	Fix

App not ready quickly enough	Increase initialDelaySeconds

Wrong path or port	Verify correct health check path and container port

Endpoint returns non-2xx	Update app to return HTTP 200 when ready

Resource limits too low	Increase CPU/memory requests/limits

Slow response	Increase timeoutSeconds or tune periodSeconds

**🔥 2. Liveness Probe Failure**

❗ What happens:
Pod is automatically restarted by OpenShift.

🛠️ Common Causes & Fixes:

Cause	Fix
Temporary app spike or slowness	Increase failureThreshold or timeoutSeconds
App enters a bad state	Fix app logic to recover or expose better health endpoint
Wrong probe endpoint	Check the HTTP/TCP command or path is accessible
Port misconfiguration	Ensure the container port is exposed and correct
Heavy GC or IO wait	Optimize app performance or resources

🚀 3. Startup Probe Failure

❗ What happens:

Pod never transitions to ready, and OpenShift may restart it depending on thresholds.

🛠️ Common Causes & Fixes:

Cause	Fix

App has long init time	Increase failureThreshold and periodSeconds

Incorrect health endpoint	Validate /startup or /health path

Database or service dependency delay	Delay startup probe until dependencies are ready

CPU starvation	Increase resource requests during startup

🧪 4. Command-based Probe Failures

Probes can be exec commands (not just HTTP or TCP).

❗ What happens:

If the command exits non-zero, the probe fails.

🛠️ Common Fixes:

Cause	Fix

Command not found	Use full paths like /bin/ls, avoid aliases

Non-zero exit code	Test command manually inside container

Permission issues	Ensure the container user has access to the command

📘 Example of All Three Probes:

```
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
  timeoutSeconds: 3

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 5
  timeoutSeconds: 2

startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```
