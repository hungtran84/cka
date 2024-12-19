
# Lab: Using `Affinity` and `Anti-Affinity` to Schedule Pods to Nodes

## Objectives
- Learn how to use `Affinity` and `Anti-Affinity` to control pod placement on nodes.
- Understand how to use `Taints` and `Tolerations` for scheduling pods.

## Steps

### 1. Using `Affinity`
- Deploy the `web` and `cache` pods with affinity to ensure that a `cache` pod is co-located with a `web` pod on the same node.

```bash
kubectl apply -f deployment-affinity.yaml
deployment.apps/hello-world-web created
deployment.apps/hello-world-cache created
```

- Check the labels on the nodes, specifically looking for the `kubernetes.io/hostname`, which is used for the `topologyKey`.

```bash
kubectl describe nodes gke-gke-test-default-pool-03d0c6b2-bfk0 | grep kubernetes.io/hostname
kubernetes.io/hostname=gke-gke-test-default-pool-03d0c6b2-bfk0
```

- Confirm that both the `web` and `cache` pods are scheduled on the same node.

```bash
kubectl get pods -o wide
```

- Scale the `web` deployment and verify that the pods are distributed across available nodes.

```bash
kubectl scale deployment hello-world-web --replicas=2
deployment.apps/hello-world-web scaled

kubectl get pods -o wide
```

- Scale the `cache` deployment and ensure that the pods are scheduled on the same node as the `web` pods.

```bash
kubectl scale deployment hello-world-cache --replicas=2
deployment.apps/hello-world-cache scaled

kubectl get pods -o wide
```

> **Hometask:** How to distribute `Web` and `Cache` pods among 3 nodes while maintaining affinity?

- Clean up the resources from these deployments.

```bash
kubectl delete -f deployment-affinity.yaml
deployment.apps "hello-world-web" deleted
deployment.apps "hello-world-cache" deleted
```

### 2. Using `Anti-Affinity`
- Deploy `web` and `cache` pods again, but this time configure `anti-affinity` to ensure that no more than 1 `web` pod is scheduled on each node.

```bash
kubectl apply -f deployment-antiaffinity.yaml
deployment.apps/hello-world-web created
deployment.apps/hello-world-cache created
```

- Scale the `web` deployment to 4 replicas. One pod will remain pending because of the `anti-affinity` rule.

```bash
kubectl scale deployment hello-world-web --replicas=4
deployment.apps/hello-world-web scaled
```

- Verify that one pod is pending due to the `anti-affinity` rule.

```bash
kubectl get pods -o wide --selector app=hello-world-web
```

- To resolve this, change the scheduling rule to `preferredDuringSchedulingIgnoredDuringExecution` and scale the deployment again.

```bash
kubectl apply -f deployment-antiaffinity-corrected.yaml
deployment.apps/hello-world-web configured
deployment.apps/hello-world-cache unchanged

kubectl scale deployment hello-world-web --replicas=4
deployment.apps/hello-world-web scaled
```

- Verify that all 4 pods are running as expected.

```bash
kubectl get pods -o wide --selector app=hello-world-web
```

- Clean up the resources.

```bash
kubectl delete -f deployment-antiaffinity-corrected.yaml
deployment.apps "hello-world-web" deleted
deployment.apps "hello-world-cache" deleted
```

### 3. Controlling Pods Placement with `Taints` and `Tolerations`

- **Add a `Taint` to the First Node**:
  To prevent Pods from being scheduled on a node, you can add a taint to that node.
  ```shell
  kubectl taint nodes gke-gke-test-default-pool-03d0c6b2-bfk0 key=MyTaint:NoSchedule
  node/gke-gke-test-default-pool-03d0c6b2-bfk0 tainted
  ```

- **Verify the Taint**:
  You can see the taint applied to the node by checking the `Taints` section.
  ```shell
  kubectl describe node gke-gke-test-default-pool-03d0c6b2-bfk0 | grep Taint
  Taints:             key=MyTaint:NoSchedule
  ```

- **Create a Deployment with 3 Replicas**:
  Create a deployment with 3 replicas.
  ```shell
  kubectl apply -f deployment.yaml
  deployment.apps/hello-world created
  ```

- **Verify Pod Placement**:
  Since the first node has a taint, Pods will be scheduled on the non-tainted nodes.
  ```shell
  kubectl get pods -o wide
  NAME                           READY   STATUS    RESTARTS   AGE   IP            NODE                                      NOMINATED NODE   READINESS GATES
  hello-world-68c787c876-mgb2k   1/1     Running   0          22s   10.28.1.54    gke-gke-test-default-pool-03d0c6b2-n7l2   <none>           <none>
  hello-world-68c787c876-ss5fl   1/1     Running   0          22s   10.28.0.118   gke-gke-test-default-pool-03d0c6b2-sv8f   <none>           <none>
  hello-world-68c787c876-v752b   1/1     Running   0          22s   10.28.0.117   gke-gke-test-default-pool-03d0c6b2-sv8f   <none>           <none>
  ```

- **Create a Deployment with a `Toleration`**:
  If you add a `Toleration` to the deployment, Pods can be scheduled on the tainted node.
  ```shell
  kubectl apply -f deployment-tolerations.yaml
  deployment.apps/hello-world-tolerations created
  ```

- **Verify Pod Distribution**:
  With the `Toleration` in place, Pods will be spread out across all 3 nodes.
  ```shell
  kubectl get pods -o wide
  NAME                                       READY   STATUS    RESTARTS   AGE     IP            NODE                                      NOMINATED NODE   READINESS GATES
  hello-world-68c787c876-mgb2k               1/1     Running   0          2m22s   10.28.1.54    gke-gke-test-default-pool-03d0c6b2-n7l2   <none>           <none>
  hello-world-68c787c876-ss5fl               1/1     Running   0          2m22s   10.28.0.118   gke-gke-test-default-pool-03d0c6b2-sv8f   <none>           <none>
  hello-world-68c787c876-v752b               1/1     Running   0          2m22s   10.28.0.117   gke-gke-test-default-pool-03d0c6b2-sv8f   <none>           <none>
  hello-world-tolerations-59ff95b965-8sflv   1/1     Running   0          75s     10.28.2.55    gke-gke-test-default-pool-03d0c6b2-bfk0   <none>           <none>
  hello-world-tolerations-59ff95b965-mttsr   1/1     Running   0          75s     10.28.1.55    gke-gke-test-default-pool-03d0c6b2-n7l2   <none>           <none>
  hello-world-tolerations-59ff95b965-t9pb7   1/1     Running   0          75s     10.28.0.119   gke-gke-test-default-pool-03d0c6b2-sv8f   <none>           <none>
  ```

- **Remove the `Taint`**:
  After testing, you can remove the taint from the node.
  ```shell
  kubectl taint nodes gke-gke-test-default-pool-03d0c6b2-bfk0 key:NoSchedule-
  node/gke-gke-test-default-pool-03d0c6b2-bfk0 untainted
  ```

- **Clean Up**:
  Finally, clean up by deleting the deployments created during the demo.
  ```shell
  kubectl delete -f deployment-tolerations.yaml
  kubectl delete -f deployment.yaml
  ```


### 3. Controlling Pods Placement with `Taints` and `Tolerations`
- Add a `Taint` to a node to control pod placement.

```bash
kubectl taint nodes gke-gke-test-default-pool-03d0c6b2-bfk0 key=MyTaint:NoSchedule
node/gke-gke-test-default-pool-03d0c6b2-bfk0 tainted
```

- Verify the `Taint` on the node.

```bash
kubectl describe node gke-gke-test-default-pool-03d0c6b2-bfk0 | grep Taint
Taints:             key=MyTaint:NoSchedule
```

- Create a deployment with 3 replicas and verify that pods are scheduled on non-tainted nodes.

```bash
kubectl apply -f deployment.yaml
deployment.apps/hello-world created
```

- Verify the pod distribution.

```bash
kubectl get pods -o wide
```

- Create a deployment with a `Toleration` to allow scheduling on the tainted node.

```bash
kubectl apply -f deployment-tolerations.yaml
deployment.apps/hello-world-tolerations created
```

- Verify that the pods are spread across all 3 nodes.

```bash
kubectl get pods -o wide
```

Certainly! Here's the complete section with the added instructions on using labels to schedule pods to nodes.

```markdown
### 4. Using `Labels` to Schedule `Pods` to `Nodes`

- Query our labels to see if the `disk` and `hardware` labels are already in place.
  ```bash
  kubectl get node -L disk,hardware
  NAME                                      STATUS   ROLES    AGE   VERSION           DISK        HARDWARE
  gke-gke-test-default-pool-03d0c6b2-bfk0   Ready    <none>   9d    v1.27.3-gke.100   local_ssd   
  gke-gke-test-default-pool-03d0c6b2-n7l2   Ready    <none>   9d    v1.27.3-gke.100               local_gpu
  gke-gke-test-default-pool-03d0c6b2-sv8f   Ready    <none>   9d    v1.27.3-gke.100               
  ```

- Otherwise, label our nodes with `disk=local_ssd` and `hardware=local_gpu`
  ```bash
  # node 1
  kubectl label node gke-gke-test-default-pool-03d0c6b2-bfk0 disk=local_ssd

  # node 2
  kubectl label node gke-gke-test-default-pool-03d0c6b2-n7l2 hardware=local_gpu
  ```

- Create 3 Pods, two using `nodeSelector`, one without.
  ```bash
  kubectl apply -f DeploymentsToNodes.yaml
  deployment.apps/hello-world-gpu created
  deployment.apps/hello-world-ssd created
  deployment.apps/hello-world created
  ```

- View the scheduling of the pods in the cluster.
  ```bash
  kubectl get node -L disk,hardware
  NAME                                      STATUS   ROLES    AGE   VERSION           DISK        HARDWARE
  gke-gke-test-default-pool-03d0c6b2-bfk0   Ready    <none>   9d    v1.27.3-gke.100   local_ssd   
  gke-gke-test-default-pool-03d0c6b2-n7l2   Ready    <none>   9d    v1.27.3-gke.100               local_gpu
  gke-gke-test-default-pool-03d0c6b2-sv8f   Ready    <none>   9d    v1.27.3-gke.100    

  kubectl get pods -o wide
  NAME                               READY   STATUS    RESTARTS   AGE   IP            NODE                                      NOMINATED NODE   READINESS GATES
  hello-world-68c787c876-wbgxq       1/1     Running   0          25s   10.28.0.120   gke-gke-test-default-pool-03d0c6b2-sv8f   <none>           <none>
  hello-world-gpu-7796849f94-nsspz   1/1     Running   0          26s   10.28.1.56    gke-gke-test-default-pool-03d0c6b2-n7l2   <none>           <none>
  hello-world-ssd-6dfb7cf77-m59mg    1/1     Running   0          26s   10.28.2.56    gke-gke-test-default-pool-03d0c6b2-bfk0   <none>           <none>
  ```

- If we scale this Deployment, all new Pods will go onto the node with the GPU label.
  ```bash
  kubectl scale deployment hello-world-gpu --replicas=3
  deployment.apps/hello-world-gpu scaled

  kubectl get pods -o wide
  NAME                               READY   STATUS    RESTARTS   AGE    IP            NODE                                      NOMINATED NODE   READINESS GATES
  hello-world-68c787c876-wbgxq       1/1     Running   0          102s   10.28.0.120   gke-gke-test-default-pool-03d0c6b2-sv8f   <none>           <none>
  hello-world-gpu-7796849f94-6f2xn   1/1     Running   0          10s    10.28.1.58    gke-gke-test-default-pool-03d0c6b2-n7l2   <none>           <none>
  hello-world-gpu-7796849f94-bxp4v   1/1     Running   0          10s    10.28.1.57    gke-gke-test-default-pool-03d0c6b2-n7l2   <none>           <none>
  hello-world-gpu-7796849f94-nsspz   1/1     Running   0          103s   10.28.1.56    gke-gke-test-default-pool-03d0c6b2-n7l2   <none>           <none>
  hello-world-ssd-6dfb7cf77-m59mg    1/1     Running   0          103s   10.28.2.56    gke-gke-test-default-pool-03d0c6b2-bfk0   <none>           <none>
  ```

- If we scale this Deployment, all new Pods will go onto the node with the SSD label.
  ```bash
  kubectl scale deployment hello-world-ssd --replicas=3 
  kubectl get pods -o wide
  ```

- If we scale this Deployment, all new Pods will likely go onto the node without the labels to keep the load balanced.
  ```bash
  kubectl scale deployment hello-world --replicas=3
  kubectl get pods -o wide 
  ```

- If we go beyond that, it will use all nodes to keep load even globally.
  ```bash
  kubectl scale deployment hello-world --replicas=10
  kubectl get pods -o wide 
  ```

- Clean up when we're finished.
  ```bash
  kubectl delete deployments.apps hello-world
  kubectl delete deployments.apps hello-world-gpu
  kubectl delete deployments.apps hello-world-ssd
  ```


## Summary
- In this lab, you learned how to use `Affinity` and `Anti-Affinity` to control pod placement on nodes based on specific rules.
- You also learned how to manage pod placement using `Taints` and `Tolerations`, ensuring that pods are scheduled on the right nodes according to their constraints.
