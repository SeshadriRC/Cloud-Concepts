**Cluster operators**

`` oc get co ``

**Pod**

``oc get pods``

``oc exec -it [pod_name] -- /bin/sh``

``oc delete pod <pod_name> -n <namespace_name> --force=true --grace-period=0``

``oc logs <pod-name> -n <namespace>``

``oc logs <pod-name> -c <container-name> -n <namespace>``

`` oc logs -f <pod-name> -n <namespace> ``

**PVC**

``oc describe pvc <pvc_name>``
