# Lab: Working with etcd

## Objectives
1. Understand the configuration of a locally hosted etcd running in a static pod.
2. Perform a backup of etcd data.
3. Restore etcd data from a backup.
4. Validate the restoration process and clean up resources.

## Prerequisites
- To work with `etcd`, we need to access to the `cp1` node.

## Steps

### Step 1: Inspect etcd Configuration
1. **Review the pod configuration:**
   ```bash
   kubectl describe pod <etcd-pod> -n kube-system
   ```
2. **Examine the static pod manifest:**
   ```bash
   sudo more /etc/kubernetes/manifests/etcd.yaml
   ```
3. **Check runtime values:**
   ```bash
   ps -aux | grep etcd
   ```

---

### Step 2: Install `etcdctl`
1. **Find etcd version:**
   ```bash
   kubectl exec -it <etcd-pod> -n kube-system -- /bin/sh -c 'ETCDCTL_API=3 /usr/local/bin/etcd --version' | head
   ```
2. **Download and install `etcdctl`:**
   ```bash
   export RELEASE="3.5.6" # Replace with your etcd version
   wget https://github.com/etcd-io/etcd/releases/download/v${RELEASE}/etcd-v${RELEASE}-linux-amd64.tar.gz
   tar -zxvf etcd-v${RELEASE}-linux-amd64.tar.gz
   cd etcd-v${RELEASE}-linux-amd64
   sudo cp etcdctl /usr/local/bin
   ```
3. **Verify installation:**
   ```bash
   ETCDCTL_API=3 etcdctl --help | head
   ```

---

### Step 3: Backup etcd Data
1. **Create a test secret:**
   ```bash
   kubectl create secret generic test-secret \
       --from-literal=username='svcaccount' \
       --from-literal=password='S0mthingS0Str0ng!'
   ```
2. **Define the etcd endpoint:**
   ```bash
   ENDPOINT=https://127.0.0.1:2379
   ```
3. **Verify cluster connection:**
   ```bash
   sudo ETCDCTL_API=3 etcdctl --endpoints=$ENDPOINT \
       --cacert=/etc/kubernetes/pki/etcd/ca.crt \
       --cert=/etc/kubernetes/pki/etcd/server.crt \
       --key=/etc/kubernetes/pki/etcd/server.key \
       member list
   ```
4. **Take a snapshot backup:**
   ```bash
   sudo ETCDCTL_API=3 etcdctl --endpoints=$ENDPOINT \
       --cacert=/etc/kubernetes/pki/etcd/ca.crt \
       --cert=/etc/kubernetes/pki/etcd/server.crt \
       --key=/etc/kubernetes/pki/etcd/server.key \
       snapshot save /var/lib/dat-backup.db
   ```
5. **Inspect snapshot metadata:**
   ```bash
   sudo ETCDCTL_API=3 etcdctl --write-out=table snapshot status /var/lib/dat-backup.db
   ```

---

### Step 4: Restore etcd Data
1. **Delete the test secret:**
   ```bash
   kubectl delete secret test-secret
   ```
2. **Restore from snapshot:**
   ```bash
   sudo ETCDCTL_API=3 etcdctl snapshot restore /var/lib/dat-backup.db
   ```
3. **Verify restored data:**
   ```bash
   sudo ls -l
   ```
4. **Move old data and replace with restored data:**
   ```bash
   sudo mv /var/lib/etcd /var/lib/etcd.OLD
   sudo mv ./default.etcd /var/lib/etcd
   ```
5. **Restart the static pod:**
   ```bash
   sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock ps | grep etcd
   CONTAINER_ID=$(sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock ps | grep etcd | awk '{ print $1 }')
   sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock stop $CONTAINER_ID
   ```
6. **Verify restoration:**
   ```bash
   kubectl get secret test-secret
   ```

---

### Step 5: Alternative Restore Method
1. **Delete the test secret again:**
   ```bash
   kubectl delete secret test-secret
   ```
2. **Restore to a specific data directory:**
   ```bash
   sudo ETCDCTL_API=3 etcdctl snapshot restore /var/lib/dat-backup.db --data-dir=/var/lib/etcd-restore
   ```
3. **Update static pod manifest:**
   Edit `/etc/kubernetes/manifests/etcd.yaml`:
   ```yaml
   - --data-dir=/var/lib/etcd-restore
   ...
   volumeMounts:
   - mountPath: /var/lib/etcd-restore
   ...
   volumes:
   - hostPath:
       name: etcd-data
       path: /var/lib/etcd-restore
   ```
   Apply the changes:
   ```bash
   sudo cp /etc/kubernetes/manifests/etcd.yaml .
   sudo vi /etc/kubernetes/manifests/etcd.yaml
   ```
4. **Verify container runtime:**
   ```bash
   sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock ps
   ```
5. **Check for restored secret:**
   ```bash
   kubectl get secret test-secret
   ```

---

### Step 6: Clean Up
1. **Remove resources and revert changes:**
   ```bash
   kubectl delete secret test-secret
   sudo cp etcd.yaml /etc/kubernetes/manifests/
   sudo rm /var/lib/dat-backup.db
   sudo rm /usr/local/bin/etcdctl
   sudo rm -rf /var/lib/etcd.OLD
   sudo rm -rf /var/lib/etcd-restore
   rm etcd-v${RELEASE}-linux-amd64.tar.gz
   ```

---

## Lab Summary
In this lab, you:
- Explored etcd configuration.
- Performed a snapshot backup of etcd data.
- Restored etcd data from the backup using two methods.
- Validated the restoration process by retrieving deleted resources.
