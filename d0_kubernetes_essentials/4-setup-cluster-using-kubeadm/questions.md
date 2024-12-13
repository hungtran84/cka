# Docker, Containers, and Kubernetes: Multiple Choice Questions

## Docker Basics

### Question 1: What is Docker?
- [ ] A programming language for creating containerized applications  
- [ ] A version control system  
- [ ] A type of virtualization hypervisor  
<details>
<summary>Show Answer</summary>
- [x] A platform for developing, shipping, and running containerized applications  
</details>

### Question 2: Which of the following commands is used to build a Docker image?
- [ ] `docker run`  
- [ ] `docker pull`  
- [ ] `docker commit`  
<details>
<summary>Show Answer</summary>
- [x] `docker build`  
</details>

### Question 3: What is the purpose of the `Dockerfile`?
- [ ] It is a configuration file for Kubernetes  
- [ ] It is a file for managing container logs  
- [ ] It is a file used to compose multiple Docker containers  
<details>
<summary>Show Answer</summary>
- [x] It is a script containing instructions to build a Docker image  
</details>

---

## Comparing Containers and Virtual Machines

### Question 4: Which of the following is a key difference between a container and a virtual machine?
- [ ] Containers include a full guest operating system  
- [ ] Virtual machines cannot run on the cloud  
- [ ] Virtual machines require less system overhead than containers  
<details>
<summary>Show Answer</summary>
- [x] Containers share the host OS kernel, while VMs run separate guest OS instances  
</details>

### Question 5: How does resource usage differ between containers and virtual machines?
- [ ] Virtual machines use less CPU and memory than containers  
- [ ] Containers require more storage than virtual machines  
- [ ] Virtual machines always run faster than containers  
<details>
<summary>Show Answer</summary>
- [x] Containers are more lightweight because they don’t include a guest operating system  
</details>

### Question 6: Which of the following is TRUE about containers and virtual machines?
- [ ] Containers use hypervisors for resource isolation  
- [ ] Virtual machines provide faster startup times than containers  
- [ ] Virtual machines are better suited for running microservices applications  
<details>
<summary>Show Answer</summary>
- [x] Containers are more portable across environments than virtual machines  
</details>

### Question 7: Why are containers often preferred over virtual machines for modern applications?
- [ ] Containers provide better security than virtual machines  
- [ ] Containers are easier to deploy but require a full guest OS  
- [ ] Containers can’t run on cloud environments  
<details>
<summary>Show Answer</summary>
- [x] Containers start quickly and consume fewer resources  
</details>

### Question 8: Which of the following is NOT a characteristic of virtual machines?
- [ ] Each VM includes a guest operating system  
- [ ] VMs are isolated using a hypervisor  
- [ ] VMs are more resource-intensive than containers  
<details>
<summary>Show Answer</summary>
- [x] VMs share the host OS kernel with other VMs  
</details>

### Question 9: In which scenario would you choose a virtual machine over a container?
- [ ] When you need fast startup times and lightweight deployment  
- [ ] When running a distributed microservices application  
- [ ] When you require portability across different OS environments  
<details>
<summary>Show Answer</summary>
- [x] When you need to run multiple applications with different operating systems  
</details>

### Question 10: Which layer is responsible for isolating virtual machines?
- [ ] Host operating system kernel  
- [ ] Container runtime  
- [ ] Guest operating system  
<details>
<summary>Show Answer</summary>
- [x] Hypervisor  
</details>

---

## Kubernetes Architecture

### Question 11: What is the primary responsibility of the Kubernetes control plane?
- [ ] To execute containers directly on the worker nodes  
- [ ] To store container images  
- [ ] To expose services to external users  
<details>
<summary>Show Answer</summary>
- [x] To manage the cluster state and ensure the desired state matches the actual state  
</details>

### Question 12: Which of the following is NOT a component of the Kubernetes control plane?
- [ ] `kube-apiserver`  
- [ ] `etcd`  
- [ ] `kube-scheduler`  
<details>
<summary>Show Answer</summary>
- [x] `kube-proxy`  
</details>

### Question 13: What is the role of `etcd` in the Kubernetes architecture?
- [ ] To manage network communication between pods  
- [ ] To distribute container images to the worker nodes  
- [ ] To handle scheduling of pods on nodes  
<details>
<summary>Show Answer</summary>
- [x] To store the cluster’s state data persistently  
</details>

### Question 14: Which component of the control plane is responsible for scheduling pods on nodes?
- [ ] `kube-controller-manager`  
- [ ] `kube-apiserver`  
- [ ] `kubelet`  
<details>
<summary>Show Answer</summary>
- [x] `kube-scheduler`  
</details>

### Question 15: What is the purpose of the `kube-apiserver`?
- [ ] To store Kubernetes logs  
- [ ] To deploy containers to nodes  
- [ ] To handle internal networking  
<details>
<summary>Show Answer</summary>
- [x] To provide an interface for communication between users and the cluster  
</details>

### Question 16: Which component of the control plane monitors and enforces the cluster's desired state?
- [ ] `kube-scheduler`  
- [ ] `etcd`  
- [ ] `kubelet`  
<details>
<summary>Show Answer</summary>
- [x] `kube-controller-manager`  
</details>

---

## Kubernetes Nodes

### Question 17: What is a Kubernetes node?
- [ ] A container runtime environment  
- [ ] A component of the control plane that manages pods  
- [ ] A namespace for organizing Kubernetes resources  
<details>
<summary>Show Answer</summary>
- [x] A physical or virtual machine that runs workloads (pods) in a Kubernetes cluster  
</details>

### Question 18: What is the function of the `kubelet` on a Kubernetes node?
- [ ] To handle networking between containers  
- [ ] To manage cluster state  
- [ ] To distribute container images  
<details>
<summary>Show Answer</summary>
- [x] To ensure that containers are running as defined in the pod specifications  
</details>

### Question 19: What is the purpose of the `kube-proxy` on a Kubernetes node?
- [ ] To run and monitor pods on the node  
- [ ] To deploy the control plane components on worker nodes  
- [ ] To store the cluster state  
<details>
<summary>Show Answer</summary>
- [x] To manage network communication and routing to and from pods  
</details>

### Question 20: Which of the following is a responsibility of the container runtime on a node?
- [ ] Managing cluster state  
- [ ] Scheduling pods on nodes  
- [ ] Exposing Kubernetes APIs  
<details>
<summary>Show Answer</summary>
- [x] Pulling container images and starting containers  
</details>
