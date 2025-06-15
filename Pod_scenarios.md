**1. Load is not equally distributed between pods**

User need to do application performance testing

**2. Pods not Schedulable**

It is because of label issue, particular label not added 

**3. Timeout issue in Pod**. 

The node that this pod was running has many no. of nodes with liveness and readiness probe failure. so we cordoned the specific node, it will not affect the existing pods. It prevents the new pod being scheduled on the particular node. Prior to this a new node was scaled up in the same machineset . then we deleted the problematic pod, post that automatically it got scheduled on new node.
