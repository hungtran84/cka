# Lab: Kubernetes Metric Server

## Objectives
- Install the Kubernetes Metric Server.
- Configure the Metric Server for secure communication and preferred address types.
- Verify the Metric Server functionality by monitoring resource metrics.
- Deploy and observe workload resource utilization.
- Clean up resources.

## Steps

### Step 1: Download the Metric Server Manifest
- Download the Metrics Server deployment manifest. The release version may change, so check for newer versions [here](https://github.com/kubernetes-sigs/metrics-server).
```bash
wget https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.6.4/components.yaml
```

### Step 2: Configure the Metric Server
- Add the following lines to the Metric Server container arguments in the manifest (`components.yaml`), around line 132:
```yaml
        - --kubelet-insecure-tls
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
```

- Example of the updated `Deployment` manifest:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: metrics-server
  name: metrics-server
  namespace: kube-system
spec:
  selector:
    matchLabels:
      k8s-app: metrics-server
  strategy:
    rollingUpdate:
      maxUnavailable: 0
  template:
    metadata:
      labels:
        k8s-app: metrics-server
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls
        image: registry.k8s.io/metrics-server/metrics-server:v0.6.4
        imagePullPolicy: IfNotPresent
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /livez
            port: https
            scheme: HTTPS
          periodSeconds: 10
        name: metrics-server
```

### Step 3: Deploy the Metric Server
```bash
kubectl apply -f components.yaml
```

### Step 4: Verify the Metric Server
- Check if the Metric Server is running:
```bash
kubectl get pods --namespace kube-system
```

- Test if the Metric Server is collecting data:
```bash
kubectl top nodes
```

- Troubleshoot any issues by inspecting the logs:
```bash
kubectl logs --namespace kube-system -l k8s-app=metrics-server
```

### Step 5: Deploy and Observe Workloads
- Deploy a pod that consumes significant CPU:
```bash
kubectl apply -f cpuburner.yaml
```

- Create a deployment and scale it:
```bash
kubectl create deployment nginx --image=nginx
kubectl scale deployment nginx --replicas=3
```

- Check if the pods are up and running:
```bash
kubectl get pods -o wide
```

- Monitor CPU and memory utilization:
```bash
kubectl top nodes
kubectl top pods
```

- Query metrics for specific pods using labels:
```bash
kubectl top pods -l app=cpuburner
```

- Sort pods by CPU or memory usage:
```bash
kubectl top pods --sort-by=cpu
kubectl top pods --sort-by=memory
```

- View container-level performance within pods:
```bash
kubectl top pods --containers
```

### Step 6: Clean Up Resources
- Delete the workloads:
```bash
kubectl delete deployment cpuburner
kubectl delete deployment nginx
```

- Remove the Metric Server and its configuration:
```bash
kubectl delete -f components.yaml
```

## Summary
In this lab, you installed and configured the Kubernetes Metric Server, tested its functionality, and observed resource metrics for workloads. You also explored filtering, sorting, and container-level performance metrics. Finally, you cleaned up all resources.
