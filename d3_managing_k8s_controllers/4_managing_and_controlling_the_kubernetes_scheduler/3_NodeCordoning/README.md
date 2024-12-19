# Node Cordoning

## Objectives
- Learn how to cordon and uncordon nodes in Kubernetes.
- Understand how pod scheduling behaves when nodes are cordoned and drained.
- Learn how to manually schedule pods to specific nodes.

## Steps

### Create a Deployment with 3 Replicas
First, apply the deployment configuration with 3 replicas:
```bash
kubectl apply -f deployment.yaml
```

### Verify Pod Distribution Across Nodes
Check the pods and their distribution across the nodes:
```bash
kubectl get pods -o wide
```

### Cordon Node 3
Cordon node `gke-gke-test-default-pool-03d0c6b2-sv8f` to prevent any new pods from being scheduled on it:
```bash
kubectl cordon gke-gke-test-default-pool-03d0c6b2-sv8f
```
Output:
```
node/gke-gke-test-default-pool-03d0c6b2-sv8f cordoned
```

### Verify Pods After Cordon
Check that no pods have been evicted from node 3:
```bash
kubectl get pods -o wide
```

### Scale the Deployment
Scale the deployment to 6 replicas:
```bash
kubectl scale deployment hello-world --replicas=6
```

### Verify New Pod Distribution
Note that node 3 won't get any new pods. One of the other nodes will now have an extra pod:
```bash
kubectl get pods -o wide
```

### Drain Node 3
Attempt to drain node 3. This will try to evict pods, but you may encounter errors due to DaemonSets and local storage:
```bash
kubectl drain gke-gke-test-default-pool-03d0c6b2-sv8f
```
Error:
```
error: unable to drain node "gke-gke-test-default-pool-03d0c6b2-sv8f" due to error: [cannot delete DaemonSet-managed Pods...]
```

### Drain Node 3 with `--ignore-daemonsets`
Since DaemonSet-managed pods can't be deleted directly, use the `--ignore-daemonsets` flag to bypass them:
```bash
kubectl drain gke-gke-test-default-pool-03d0c6b2-sv8f --ignore-daemonsets
```
Error:
```
error: unable to drain node "gke-gke-test-default-pool-03d0c6b2-sv8f" due to error: [cannot delete Pods with local storage...]
```

### Drain Node 3 with `--delete-emptydir-data`
Work around local storage issues by adding the `--delete-emptydir-data` flag:
```bash
kubectl drain gke-gke-test-default-pool-03d0c6b2-sv8f --ignore-daemonsets --delete-emptydir-data
```
Output:
```
node/gke-gke-test-default-pool-03d0c6b2-sv8f drained
```

### Verify Pods After Drain
Check that all the pods are now scheduled on nodes 1 and 2:
```bash
kubectl get pods -o wide
```

### Uncordon Node 3
Uncordon node 3, allowing pods to be scheduled again:
```bash
kubectl uncordon gke-gke-test-default-pool-03d0c6b2-sv8f
```
Output:
```
node/gke-gke-test-default-pool-03d0c6b2-sv8f uncordoned
```

### Scale the Deployment Again
Scale the deployment to 9 replicas, which will result in some of the new pods being scheduled on node 3:
```bash
kubectl scale deployment hello-world --replicas=9
```

### Verify Pod Scheduling After Scaling
Check that all 3 nodes now have pods scheduled:
```bash
kubectl get pods -o wide
```

### Clean Up Resources
Delete the deployment to clean up the resources:
```bash
kubectl delete deployment hello-world
```

---

### Manually Scheduling a Pod by `nodeName`
To manually schedule a pod on a specific node, apply a pod configuration:
```bash
kubectl apply -f pod.yaml
```

### Verify Pod Scheduling on Node 3
Check that the pod is scheduled on node 3:
```bash
kubectl get pod -o wide
```

### Delete the Pod
Since the pod is unmanaged, it won't be recreated:
```bash
kubectl delete pod hello-world-pod
```

### Cordon Node 3 Again
Cordon node 3 again:
```bash
kubectl cordon gke-gke-test-default-pool-03d0c6b2-sv8f
```

### Attempt to Recreate the Pod
Reapply the pod configuration:
```bash
kubectl apply -f pod.yaml
```

### Verify Pod Scheduling is Disabled
The pod status will show as `SchedulingDisabled` because the node is cordoned:
```bash
kubectl get pod -o wide
```

### Drain Node 3 with Force Flag
Attempt to drain the node with the `--force` flag, as the pod is unmanaged:
```bash
kubectl drain gke-gke-test-default-pool-03d0c6b2-sv8f --ignore-daemonsets
```
Error:
```
error: unable to drain node "gke-gke-test-default-pool-03d0c6b2-sv8f" due to error: [cannot delete Pods declare no controller...]
```

### Clean Up the Demo
Delete the pod and uncordon the node to return the node to normal scheduling:
```bash
kubectl delete pod hello-world-pod
kubectl uncordon gke-gke-test-default-pool-03d0c6b2-sv8f
```

## Summary
In this lab, you learned how to:
- Cordon and uncordon nodes to control pod scheduling.
- Drain nodes, handling challenges like DaemonSets and local storage.
- Manually schedule pods and handle pod evictions when nodes are cordoned.
