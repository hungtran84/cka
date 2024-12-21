# Lab: Investigating and Configuring Kubernetes DNS

## Lab Objectives
1. Investigate the default Cluster DNS Service setup.
2. Customize CoreDNS forwarders and validate changes.
3. Configure Pod DNS client settings.
4. Explore DNS A records for Pods and Services.

---

## Investigating the Cluster DNS Service

### Step 1: Inspect the CoreDNS Service
CoreDNS is deployed as a `Service` in the `kube-system` namespace. Run the following command to list the services:

```bash
kubectl get service --namespace kube-system
```

### Step 2: Examine the CoreDNS Deployment
CoreDNS runs as a `Deployment` with 2 replicas. It uses a `ConfigMap` for its configuration, mounted as a volume.

```bash
kubectl describe deployment coredns --namespace kube-system
```

Details include:
- **Replicas**: 2
- **Args**: Configuration file (`/etc/coredns/Corefile`) backed by a `ConfigMap`.
- **Ports**: 53/UDP, 53/TCP, 9153/TCP.
- **Health Checks**: Liveness and readiness probes.
- **Volumes**: ConfigMap named `coredns`.

---

## Configuring CoreDNS to Use Custom Forwarders

### Step 1: Update CoreDNS Configuration
Modify the CoreDNS `ConfigMap` to use custom forwarders:
- Replace `forward . /etc/resolv.conf` with `forward . 1.1.1.1`.
- Add a conditional forwarder for specific domains.

Apply the updated configuration:

```bash
kubectl apply -f CoreDNSConfigCustom.yaml --namespace kube-system
```

### Step 2: Verify Configuration Updates
Monitor logs to confirm CoreDNS reloads its configuration. This may take a minute or two:

```bash
kubectl logs --namespace kube-system --selector 'k8s-app=kube-dns' --follow
```

### Step 3: Test DNS Queries
Verify DNS resolution using the cluster IP of the DNS service:

```bash
SERVICEIP=$(kubectl get service --namespace kube-system kube-dns -o jsonpath='{ .spec.clusterIP }')
nslookup hungtran.com $SERVICEIP
```

### Step 4: Restore Default Configuration
Revert to the default configuration (`forward . /etc/resolv.conf`):

```bash
kubectl apply -f CoreDNSConfigDefault.yaml --namespace kube-system
```

---

## Configuring Pod DNS Client Configuration

### Step 1: Apply Custom DNS Settings
Deploy a configuration for Pods with custom DNS settings:

```bash
kubectl apply -f DeploymentCustomDns.yaml
```

### Step 2: Inspect Pod DNS Settings
Check the DNS configuration of a Pod created with the custom settings:

```bash
PODNAME=$(kubectl get pods --selector=app=hello-world-customdns -o jsonpath='{ .items[0].metadata.name }')
echo $PODNAME
kubectl exec -it $PODNAME -- cat /etc/resolv.conf
```

### Step 3: Clean Up Resources
Delete the custom deployment:

```bash
kubectl delete -f DeploymentCustomDns.yaml
```

---

## Exploring Pod and Service DNS A Records

### Step 1: Create a Deployment and Service
Apply the deployment and service configuration:

```bash
kubectl apply -f Deployment.yaml
```

### Step 2: Get Pod Details
List Pods and their IP addresses:

```bash
kubectl get pods -o wide
```

### Step 3: Query Pod DNS A Record
Replace dots in the Pod IP with dashes (e.g., `192.168.206.68` becomes `192-168-206-68`), then query its DNS A record:

```bash
SERVICEIP=$(kubectl get service --namespace kube-system kube-dns -o jsonpath='{ .spec.clusterIP }')
nslookup 192-168-206-68.default.pod.cluster.local $SERVICEIP
```

### Step 4: Query Service DNS A Record
Verify the DNS A record for the service:

```bash
kubectl get service
nslookup hello-world.default.svc.cluster.local $SERVICEIP
```

### Step 5: Clean Up Resources
Delete the deployment and service:

```bash
kubectl delete -f Deployment.yaml
```

---

## Summary
In this lab, you explored the default Cluster DNS setup, customized CoreDNS forwarders, configured Pod DNS settings, and verified DNS A records for Pods and Services.
