

[Openshift issue](#Openshift-issue)
[Database issue](#Database-issue)

## Openshift issue

**Pod crashloopback off**

We observed a CrashLoopBackOff in the pod due to an OOMKilled event. We informed the application team and instructed them to allocate sufficient memory.

**Quota issue**

The application team was unable to deploy the application as it required a large resource quota, while the namespace was allocated with a medium quota. To address this, we increased the quota from medium to large. After the update, the issue was resolved.

## Database issue

**Turbonomics activity**
user initially allocated a **db.t4g.2xlarge** instance type to the database, anticipating high resource consumption. However, as the resource usage remained low over time, Turbonomic automatically downgraded the instance type to **db.t4g.large**.

**HugePages**

The database was initially running on a db.t4g.large instance type. Due to low activity, Turbonomic automatically scaled it down to a db.t4g.medium instance. Following the scale-down, the database became inaccessible due to an incompatible parameter state—hugepages was set to ON, which is not supported on the medium instance type. To resolve the issue, we set the hugepages parameter to OFF followed by a database reboot. After the reboot, the database became accessible.
