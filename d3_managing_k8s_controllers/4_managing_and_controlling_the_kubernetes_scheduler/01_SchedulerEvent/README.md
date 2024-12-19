# Kubernetes Scheduling

## Objectives
- Learn how to create a deployment and observe how Kubernetes schedules pods across nodes.
- Understand how Kubernetes scales deployments and distributes pods.
- Explore how resource requests impact pod scheduling and scaling.

## Steps

### Finding Scheduling Information

1. **Create a Deployment with 3 replicas:**
    - Apply the deployment configuration from the `deployment.yaml` file:
    ```bash
    kubectl apply -f deployment.yaml
    # Output: deployment.apps/hello-world created
    ```

2. **Check Pod distribution across nodes:**
    - Get information about the pods, including which node each pod is running on:
    ```bash
    kubectl get pods -o wide
    ```

3. **Look at Pod events to see the scheduler’s decision-making process:**
    - Describe the pod to view scheduling events:
    ```bash
    kubectl describe pod <pod-name>
    ```

4. **Scale the deployment to 6 replicas:**
    - Update the deployment to scale the number of replicas:
    ```bash
    kubectl scale deployment hello-world --replicas=6
    # Output: deployment.apps/hello-world scaled
    ```

5. **Check Pod distribution again after scaling:**
    - Verify that the pods are distributed evenly across nodes:
    ```bash
    kubectl get pods -o wide
    ```

6. **Check the node assignment for a specific pod:**
    - Get detailed information for a specific pod, including the node name:
    ```bash
    kubectl get pods hello-world-[tab][tab] -o yaml
    # Output shows:
    # nodeName: gke-gke-test-default-pool-03d0c6b2-bfk0
    ```

7. **Clean up the resources:**
    - Delete the deployment once you are done:
    ```bash
    kubectl delete deployment hello-world
    # Output: deployment.apps "hello-world" deleted
    ```

---

### Scheduling Pods with Resource Requests

1. **Start a watch to observe pod status changes:**
    - Start watching the pods as they transition through `Pending`, `ContainerCreating`, and `Running` states:
    ```bash
    kubectl get pods --watch &
    ```

2. **Create a deployment with resource requests:**
    - Apply the configuration in the `requests.yaml` file, which specifies CPU requests for each pod:
    ```bash
    kubectl apply -f requests.yaml
    # The pods will go through these states:
    # hello-world-requests-856cf9b988-xdvb2   0/1     Pending             0          0s
    # hello-world-requests-856cf9b988-pvffz   0/1     ContainerCreating   0          0s
    # hello-world-requests-856cf9b988-ghwr4   0/1     Pending             0          0s
    ```

3. **Verify pod status after applying the resource request configuration:**
    - Use `kubectl get pods` to verify the status of each pod:
    ```bash
    kubectl get pods -o wide
    ```

4. **Scale the deployment to 6 replicas:**
    - Scale the deployment and observe how some pods stay in `Pending` state:
    ```bash
    kubectl scale deployment hello-world-requests --replicas=6
    ```

    Output shows that some pods remain in the `Pending` state:
    ```bash
    hello-world-requests-7578c79cb4-t68ql   0/1     Pending             0          0s
    hello-world-requests-7578c79cb4-fnvkr   0/1     Pending             0          0s
    hello-world-requests-7578c79cb4-rs2p6   0/1     ContainerCreating   0          0s
    ```

5. **Inspect why the pods are in `Pending` state:**
    - Check the pods that are still pending and view detailed pod events to see why they cannot be scheduled:
    ```bash
    kubectl get pods -o wide | grep Pending
    kubectl describe pod hello-world-requests-7578c79cb4-fnvkr
    ```

    Example event output:
    ```bash
    Events:
    Type     Reason             Age                  From                Message
    ----     ------             ----                 ----                -------
    Warning  FailedScheduling   108s (x2 over 110s)  default-scheduler   0/3 nodes are available: 3 Insufficient cpu. preemption: 0/3 nodes are available: 3 No preemption victims found for incoming pod..
    Normal   NotTriggerScaleUp  108s                 cluster-autoscaler  pod didn't trigger scale-up:
    ```

6. **Clean up resources after the demo:**
    - Delete the deployment to remove the created pods:
    ```bash
    kubectl delete deployment hello-world-requests
    ```

7. **Stop the watch:**
    - Stop the watch by bringing the background process to the foreground and then terminating it:
    ```bash
    fg
    # Then press Ctrl+C to stop the watch
    ```

---

## Summary

In this lab, you:
- Created a deployment and scaled it to observe how Kubernetes schedules pods across nodes.
- Learned how Kubernetes schedules pods based on resource requests and how to scale deployments.
- Explored how resource constraints can lead to pods staying in a `Pending` state and how to view detailed scheduling events.

By the end of this lab, you should have a better understanding of how Kubernetes schedules pods and manages resources in a cluster.
