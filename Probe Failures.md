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
