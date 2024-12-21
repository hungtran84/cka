# Lab: Kubernetes Logs and Events

## Objectives
- Understand how to work with pod logs in Kubernetes.
- Learn to debug issues using logs from nodes, control plane, and pods.
- Gain insight into Kubernetes event management and monitoring.

---

## Steps

### 1. Pods
#### Check the logs for a single container pod
```bash
kubectl create deployment nginx --image=nginx
PODNAME=$(kubectl get pods -l app=nginx -o jsonpath='{.items[0].metadata.name}')
echo $PODNAME
kubectl logs $PODNAME
```

#### Clean up the deployment
```bash
kubectl delete deployment nginx
```

#### Create a multi-container pod
```bash
kubectl apply -f multicontainer.yaml
```

#### Get logs for specific containers in the pod
```bash
PODNAME=$(kubectl get pods -l app=loggingdemo -o jsonpath='{.items[0].metadata.name}')
echo $PODNAME
kubectl logs $PODNAME -c container1
kubectl logs $PODNAME -c container2
```

#### Retrieve logs for all containers in the pod
```bash
kubectl logs $PODNAME --all-containers
```

#### Follow logs for real-time debugging
```bash
kubectl logs $PODNAME --all-containers --follow
ctrl+c
```

#### Filter logs using selectors and save them
```bash
kubectl logs --selector app=loggingdemo --all-containers > allpods.txt
```

#### Tail the last 5 log entries
```bash
kubectl logs --selector app=loggingdemo --all-containers --tail 5
```

---

### 2. Nodes
#### Check the kubelet service status
```bash
systemctl status kubelet.service
```

#### View kubelet logs with `journalctl`
```bash
journalctl -u kubelet.service
journalctl -u kubelet.service | grep -i ERROR
journalctl -u kubelet.service --since today --no-pager
```

---

### 3. Control Plane
#### List control plane pods
```bash
kubectl get pods --namespace kube-system --selector tier=control-plane
```

#### Get logs for control plane pods
```bash
kubectl logs --namespace kube-system <kube-apiserver-pod>
```

#### Retrieve logs using `crictl`
```bash
sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock ps
CONTAINER_ID=$(sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock ps | grep kube-apiserver | awk '{print $1}')
sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock logs $CONTAINER_ID
```

#### Access logs directly from the filesystem
```bash
sudo ls /var/log/containers
sudo tail /var/log/containers/<kube-apiserver-pod>*
```

---

### 4. Events
#### Show all events
```bash
kubectl get events
```

#### Sort events by creation timestamp
```bash
kubectl get events --sort-by='.metadata.creationTimestamp'
```

#### Create a flawed deployment to generate warnings
```bash
kubectl create deployment nginx --image=ngins
kubectl get events --field-selector type=Warning
kubectl get events --field-selector type=Warning,reason=Failed
```

#### Monitor events in real-time
```bash
kubectl get events --watch &
kubectl scale deployment loggingdemo --replicas=5
fg
ctrl+c
```

#### View events in a specific namespace
```bash
kubectl get events --namespace kube-system
```

#### View events as part of `kubectl describe`
```bash
kubectl describe deployment nginx
kubectl describe replicaset nginx-646c766dc9
kubectl describe pods nginx
```

#### Clean up resources
```bash
kubectl delete -f multicontainer.yaml
kubectl delete deployment nginx
```

#### Verify events are still available
```bash
kubectl get events --sort-by='.metadata.creationTimestamp'
```

---

## Summary
- This lab covered using Kubernetes commands to retrieve logs and events for pods, nodes, and the control plane.
- Learned to debug real-time issues using log tailing and event monitoring.
- Practiced cleaning up resources while maintaining insights through retained event logs.
