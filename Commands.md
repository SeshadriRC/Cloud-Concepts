**Cluster operators**

`` oc get co ``

**Pod**

``oc get pods``

``oc exec -it [pod_name] -- /bin/sh``

``oc delete pod <pod_name> -n <namespace_name> --force=true --grace-period=0``

**PVC**

``oc describe pvc <pvc_name>``
