# Cluster Upgrade

## Objectives
- Learn how to upgrade a Kubernetes cluster from one minor version to the next.
- Understand the steps for upgrading control plane and worker nodes.
- Verify the upgraded cluster components.

---

## Steps

### Step 1: Identify the Target Version
- **Note:** Kubernetes only supports upgrades from one minor version to the next.  
Run the following commands to find the available versions:

```bash
sudo apt update
apt-cache policy kubeadm | head
```

- Determine the current version of your cluster:

```bash
kubectl version --output yaml
kubectl get nodes
```

---

### Step 2: Upgrade Control Plane Nodes

#### 2.1 Upgrade `kubeadm` on the Control Plane Node
Replace the version with the desired upgrade version:

```bash
sudo apt-mark unhold kubeadm
sudo apt-get update
sudo apt-get install -y kubeadm=1.30.6-1.1
sudo apt-mark hold kubeadm
```

#### 2.2 Drain Workloads from the Control Plane Node
```bash
kubectl drain cp1 --ignore-daemonsets
```

#### 2.3 Run Upgrade Plan and Pre-Flight Checks
```bash
sudo kubeadm upgrade plan v1.30.6
```
This command will:
- Highlight required manual updates (e.g., `kubelet`).
- Display version details for control plane components.

#### 2.4 Prepull Required Images
```bash
sudo kubeadm config images pull
```

#### 2.5 Apply the Upgrade
```bash
sudo kubeadm upgrade apply v1.30.6
```

#### 2.6 Uncordon the Control Plane Node
```bash
kubectl uncordon cp1
```

#### 2.7 Upgrade `kubelet` and `kubectl` on the Control Plane Node
```bash
sudo apt-mark unhold kubelet kubectl 
sudo apt-get update
sudo apt-get install -y kubelet=1.30.6-1.1 kubectl=1.30.6-1.1
sudo apt-mark hold kubelet kubectl
```

#### 2.8 Verify Upgrade Status
```bash
kubectl version -oyaml
kubectl get nodes
```

> **Note:** Repeat the above steps for any additional control plane nodes.

---

### Step 3: Upgrade Worker Nodes

#### 3.1 Drain Workloads from a Worker Node
```bash
kubectl drain node1 --ignore-daemonsets
```

#### 3.2 Access the Worker Node
```bash
gcloud compute ssh node1
```

#### 3.3 Upgrade `kubeadm` on the Worker Node
```bash
sudo apt-mark unhold kubeadm 
sudo apt-get update
sudo apt-get install -y kubeadm=1.30.6-1.1
sudo apt-mark hold kubeadm
```

#### 3.4 Update `kubelet` Configuration
```bash
sudo kubeadm upgrade node
```

#### 3.5 Upgrade `kubelet` and `kubectl`
```bash
sudo apt-mark unhold kubelet kubectl 
sudo apt-get update
sudo apt-get install -y kubelet=1.30.6-1.1 kubectl=1.30.6-1.1
sudo apt-mark hold kubelet kubectl
```

#### 3.6 Exit the Node
```bash
exit
```

#### 3.7 Verify Node Version
```bash
kubectl get nodes
```

#### 3.8 Uncordon the Node
```bash
kubectl uncordon node1
```

#### 3.9 Repeat for Additional Worker Nodes
Repeat the process for other worker nodes (e.g., `node2`, `node3`).

---

## Summary
- You successfully upgraded the control plane and worker nodes of the Kubernetes cluster.
- Use `kubectl get nodes` to verify the versions of all nodes post-upgrade.
