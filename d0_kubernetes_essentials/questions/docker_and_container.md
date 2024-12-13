# Docker and Container Concepts: Multiple Choice Questions

---

## Part 1: Container Concepts and Comparing Containers with VMs

### Question 1: What is a container in computing?
- A) A lightweight virtual machine that includes a full operating system  
- B) A standard unit of software that packages code and its dependencies  
- C) A hypervisor for managing virtualized environments  
- D) A physical server for hosting applications  
<details>
<summary>Reveal Answer</summary>
**Answer: B**
  
Explanation: Containers package code and dependencies together to ensure applications run consistently across environments.
</details>

### Question 2: How do containers differ from virtual machines (VMs)?
- A) Containers include the host OS kernel, while VMs do not  
- B) Containers are more resource-intensive than VMs  
- C) Containers share the host OS kernel, while VMs run their own guest OS  
- D) Containers can only run on Linux systems  
<details>
<summary>Reveal Answer</summary>
**Answer: C**  
Explanation: Containers share the host OS kernel, making them lightweight compared to VMs, which run a separate guest OS.
</details>

### Question 3: Which of the following is an advantage of using containers over virtual machines?
- A) Faster startup times  
- B) Greater hardware isolation  
- C) Higher resource consumption  
- D) Support for multiple hypervisors  
<details>
<summary>Reveal Answer</summary>
**Answer: A**  
Explanation: Containers are lightweight and can start in seconds, whereas VMs require booting a full OS.
</details>

### Question 4: What is a key characteristic of containers?
- A) Containers require a hypervisor to operate  
- B) Containers run independently of the host system’s OS  
- C) Containers virtualize the hardware  
- D) Containers package applications and dependencies together  
<details>
<summary>Reveal Answer</summary>
**Answer: D**  
Explanation: Containers encapsulate applications and their dependencies to ensure portability and consistency.
</details>

### Question 5: Which statement about containers is FALSE?
- A) Containers are lightweight compared to VMs  
- B) Containers can run multiple isolated workloads on a single host  
- C) Containers require a separate guest OS for each instance  
- D) Containers are ideal for microservices architectures  
<details>
<summary>Reveal Answer</summary>
**Answer: C**  
Explanation: Containers do not require a separate guest OS; they share the host OS kernel.
</details>

### Question 6: Which of the following is NOT a characteristic of virtual machines?
- A) VMs are isolated from each other and the host system  
- B) VMs include their own operating system  
- C) VMs share the host operating system's kernel  
- D) VMs consume more resources compared to containers  
<details>
<summary>Reveal Answer</summary>
**Answer: C**  
Explanation: VMs run their own guest OS, unlike containers, which share the host OS kernel.
</details>

### Question 7: Which of these provides the primary isolation mechanism in containers?
- A) Virtualized hardware  
- B) Operating system-level virtualization  
- C) Hypervisor  
- D) Separate virtual networks  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: Containers provide isolation through OS-level virtualization, using the same host kernel.
</details>

### Question 8: What is the primary purpose of container orchestration tools like Kubernetes?
- A) To deploy virtual machines in cloud environments  
- B) To manage and automate the deployment of containers  
- C) To build Docker images  
- D) To monitor and manage server hardware  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: Kubernetes and other orchestration tools help automate the deployment, scaling, and management of containerized applications.
</details>

### Question 9: Which is an example of an orchestration tool used to manage containers?
- A) VirtualBox  
- B) Kubernetes  
- C) VMware vSphere  
- D) Docker Hub  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: Kubernetes is a popular orchestration tool used for managing containerized applications.
</details>

### Question 10: Which of the following best describes a container image?
- A) A running instance of a containerized application  
- B) A packaged environment including an application and all its dependencies  
- C) A virtual machine running on a host OS  
- D) A repository where container logs are stored  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: A container image contains everything needed to run an application: code, libraries, dependencies, and runtime.
</details>

---

## Part 2: Docker Container in Action

### Question 11: What is the purpose of a `Dockerfile`?
- A) To compose multiple containers into a single application  
- B) To define instructions for building a Docker image  
- C) To deploy containers on a Kubernetes cluster  
- D) To monitor running containers  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: A `Dockerfile` specifies instructions for building a Docker image, including application code and dependencies.
</details>

### Question 12: Which command is used to build a Docker image?
- A) `docker run`  
- B) `docker pull`  
- C) `docker build`  
- D) `docker start`  
<details>
<summary>Reveal Answer</summary>
**Answer: C**  
Explanation: The `docker build` command creates a Docker image from a `Dockerfile`.
</details>

### Question 13: What is the purpose of the `docker run` command?
- A) To download an image from a registry  
- B) To start a container from an image  
- C) To stop a running container  
- D) To build an image from a `Dockerfile`  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: The `docker run` command creates and starts a container from a Docker image.
</details>

### Question 14: Which command lists all running Docker containers?
- A) `docker images`  
- B) `docker ps`  
- C) `docker start`  
- D) `docker inspect`  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: The `docker ps` command lists all running containers.
</details>

### Question 15: How do you remove an unused Docker image?
- A) `docker stop <image>`  
- B) `docker delete <image>`  
- C) `docker remove <image>`  
- D) `docker rmi <image>`  
<details>
<summary>Reveal Answer</summary>
**Answer: D**  
Explanation: The `docker rmi` command removes unused Docker images.
</details>

### Question 16: What does the `docker push` command do?
- A) Pull an image from Docker Hub  
- B) Push a container to a Kubernetes cluster  
- C) Upload a local image to a Docker registry  
- D) Export a container’s logs  
<details>
<summary>Reveal Answer</summary>
**Answer: C**  
Explanation: The `docker push` command uploads a local image to a container registry like Docker Hub.
</details>

### Question 17: What does the `docker logs` command do?
- A) Start a new container  
- B) Retrieve logs from a running container  
- C) Build an image from a Dockerfile  
- D) Push a container to Docker Hub  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: The `docker logs` command is used to fetch logs from a running container.
</details>

### Question 18: What is the main purpose of the `docker-compose` tool?
- A) To manage Docker images  
- B) To run multi-container Docker applications  
- C) To monitor container resource usage  
- D) To start a single container interactively  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: Docker Compose helps define and manage multi-container applications.
</details>

### Question 19: Which file is used to define services in Docker Compose?
- A) `Dockerfile`  
- B) `docker-compose.yaml`  
- C) `compose.yml`  
- D) `services.yaml`  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: The `docker-compose.yaml` file is used to define the services, networks, and volumes for Docker Compose.
</details>

### Question 20: Which command starts the services defined in a `docker-compose.yaml` file?
- A) `docker-compose build`  
- B) `docker-compose run`  
- C) `docker-compose up`  
- D) `docker-compose start`  
<details>
<summary>Reveal Answer</summary>
**Answer: C**  
Explanation: The `docker-compose up` command starts all the services defined in the Compose file.
</details>

---

## Part 3: Docker Compose

### Question 21: What does the `docker-compose down` command do?
- A) Starts the containers in the background  
- B) Stops and removes containers, networks, and volumes  
- C) Deploys the application to a Kubernetes cluster  
- D) Restarts the containers without changing configuration  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: `docker-compose down` stops and removes all containers, networks, and volumes created by Compose.
</details>

### Question 22: What is the primary purpose of `docker-compose.yml` file?
- A) To configure the Docker engine  
- B) To define and configure multi-container applications  
- C) To deploy containers on a Kubernetes cluster  
- D) To run a single container interactively  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: The `docker-compose.yml` file defines and configures multiple services that are part of a Docker application.
</details>

### Question 23: What does the `docker-compose build` command do?
- A) Pulls all required images for the services  
- B) Builds the images for the services defined in the `docker-compose.yml` file  
- C) Starts all services defined in the Compose file  
- D) Pushes the images to a Docker registry  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: The `docker-compose build` command builds images for the services as defined in the Compose file.
</details>

### Question 24: Which of these files is required for Docker Compose to run?
- A) `Dockerfile`  
- B) `docker-compose.yaml`  
- C) `docker-compose.json`  
- D) `Dockerfile.json`  
<details>
<summary>Reveal Answer</summary>
**Answer: B**  
Explanation: `docker-compose.yaml` is the configuration file required for Docker Compose to define services.
</details>


### Question 25: What happens if you run `docker-compose up` with the `-d` flag?
- A) It starts the containers in the background  
- B) It deploys the services to Kubernetes  
- C) It runs the containers interactively  
- D) It removes all stopped containers  
<details>
<summary>Reveal Answer</summary>
**Answer: A**  
Explanation: The `-d` flag runs the services in the background (detached mode).
</details>
