

[Openshift issue](#Openshift-issue)
[Database issue](#Database-issue)

## Openshift issue

**Pod crashloopback off**

We observed a CrashLoopBackOff in the pod due to an OOMKilled event. We informed the application team and instructed them to allocate sufficient memory.

**Quota issue**

The application team was unable to deploy the application as it required a large resource quota, while the namespace was allocated with a medium quota. To address this, we increased the quota from medium to large. After the update, the issue was resolved.

## Database issue

user initially allocated a **db.t4g.2xlarge** instance type to the database, anticipating high resource consumption. However, as the resource usage remained low over time, Turbonomic automatically downgraded the instance type to **db.t4g.large**.
