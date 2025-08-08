# cka
**CKA Labs**   Hands-on labs tailored for CKA learners to master Kubernetes concepts and practical tasks. Designed to help you prepare for the Certified Kubernetes Administrator exam with real-world scenarios and essential cluster management exercises.

## Table of Contents

### Domain 0: Kubernetes Essentials
- [Containerize Application](./d0_kubernetes_essentials/1-containerize-app/README.md)
- [Docker Compose](./d0_kubernetes_essentials/2-docker-compose/README.md)
- [Exploring K8s Cluster](./d0_kubernetes_essentials/3-exploring-k8s-cluster/README.md)

### Domain 1: K8s Installation & Configuration Fundamentals
- [Setup Cluster Using Kubeadm](./d1_k8s_installation_configuration_fundamentals/1-setup-cluster-using-kubeadm/README.md)
- [Create GKE Cluster](./d1_k8s_installation_configuration_fundamentals/2-create-gke-cluster/README.md)

### Domain 2: Managing API Server
#### Using K8s API
- [API Objects](./d2_managing_api_server/1_using_k8s_api/1_api_objects/README.md)
- [API Object Version](./d2_managing_api_server/1_using_k8s_api/2_api_object_version/README.md)
- [Anatomy API Request](./d2_managing_api_server/1_using_k8s_api/3_anatomy_api_request/README.md)

#### Running and Managing Pods
- [Running Pods](./d2_managing_api_server/2_running_and_managing_pods/1_running_pods/README.md)
- [Multi-Containers Pods - Ambassador Pattern](./d2_managing_api_server/2_running_and_managing_pods/2_multi_containers_pods/2a-ambassador-pattern/README.md)
- [Multi-Containers Pods - Sidecar Pattern](./d2_managing_api_server/2_running_and_managing_pods/2_multi_containers_pods/2b-sidecar-pattern/README.md)
- [Pod Lifecycle](./d2_managing_api_server/2_running_and_managing_pods/3_pod_lifecycle/README.md)
- [Probes](./d2_managing_api_server/2_running_and_managing_pods/4_probes/README.md)

#### Managing Objects with Labels, Annotations & Namespaces
- [Namespace](./d2_managing_api_server/3_managing_objects_with_labels_annotations_ns/1_namespace/README.md)
- [Labels](./d2_managing_api_server/3_managing_objects_with_labels_annotations_ns/2_labels/README.md)

### Domain 3: Managing K8s Controllers
#### Understanding Kubernetes Controllers and Workloads
- [Exploring Kube System](./d3_managing_k8s_controllers/1_understanding_kubernetes_controllers_and_workloads/1_exploring_kube_system/README.md)
- [Deployment Basics](./d3_managing_k8s_controllers/1_understanding_kubernetes_controllers_and_workloads/2_deployment_basics/README.md)
- [ReplicaSet](./d3_managing_k8s_controllers/1_understanding_kubernetes_controllers_and_workloads/3_replicaset/README.md)

#### Maintaining Applications with Deployments
- [Updating Deployment](./d3_managing_k8s_controllers/2_maintaining_applications_with_deployments/1_updating_deployment/README.md)
- [Rollback and Restart Deployment](./d3_managing_k8s_controllers/2_maintaining_applications_with_deployments/2_rollback_and_restart_deployment/README.md)
- [Control Rollout](./d3_managing_k8s_controllers/2_maintaining_applications_with_deployments/3_control_rollout/README.md)

#### Deploying and Maintaining Applications with DaemonSets and Jobs
- [DaemonSet](./d3_managing_k8s_controllers/3_deploying_and_maintaining_applications_with_daemonsets_and_jobs/1_daemonSet/README.md)
- [Job & CronJob](./d3_managing_k8s_controllers/3_deploying_and_maintaining_applications_with_daemonsets_and_jobs/2_Job_CronJob/README.md)
- [StatefulSet](./d3_managing_k8s_controllers/3_deploying_and_maintaining_applications_with_daemonsets_and_jobs/3_statefulset/README.md)

#### Managing and Controlling the Kubernetes Scheduler
- [Scheduler Event](./d3_managing_k8s_controllers/4_managing_and_controlling_the_kubernetes_scheduler/1_SchedulerEvent/README.md)
- [Scheduling](./d3_managing_k8s_controllers/4_managing_and_controlling_the_kubernetes_scheduler/2_Scheduling/README.md)
- [Node Cordoning](./d3_managing_k8s_controllers/4_managing_and_controlling_the_kubernetes_scheduler/3_NodeCordoning/README.md)

### Domain 4: Configuring & Managing K8s Storage
#### Configuring & Managing Storage
- [Static Provisioning PV](./d4_configuring_managing_k8s_storage/1_configuring_managing_storage/1_static_provisioning_pv/README.md)
- [Dynamic Provisioning](./d4_configuring_managing_k8s_storage/1_configuring_managing_storage/2_dynamic_provisioning/README.md)

#### Configuration as Data: Environment Variables, Secrets and ConfigMaps
- [Environment Variables](./d4_configuring_managing_k8s_storage/2_configuration_as_data_environment_variables_secrets_and_configmaps/1_environment_variables/README.md)
- [Secrets](./d4_configuring_managing_k8s_storage/2_configuration_as_data_environment_variables_secrets_and_configmaps/2_secrets/README.md)
- [Docker Registry Secret](./d4_configuring_managing_k8s_storage/2_configuration_as_data_environment_variables_secrets_and_configmaps/3_docker_registry_secret/README.md)
- [ConfigMap](./d4_configuring_managing_k8s_storage/2_configuration_as_data_environment_variables_secrets_and_configmaps/4_configMap/README.md)

### Domain 5: Configuring & Managing Kubernetes Networking
#### Kubernetes Networking Fundamentals
- [Configuring DNS](./d5-configuring-managing-kubernetes-networking/1_kubernetes_networking_fundamentals/1_configuring_DNS/README.md)
- [Understanding NetworkPolicy](./d5-configuring-managing-kubernetes-networking/1_kubernetes_networking_fundamentals/2_understanding_networkPolicy/README.md)

#### Configuring & Managing Services
- [Services](./d5-configuring-managing-kubernetes-networking/2_configuring_managing_services/1_services/README.md)
- [Service Discovery](./d5-configuring-managing-kubernetes-networking/2_configuring_managing_services/2_ServiceDiscovery/README.md)

#### Configuring & Managing Ingress
- [Ingress](./d5-configuring-managing-kubernetes-networking/3_configuring_managing_ingress/README.md)

### Domain 6: Maintaining, Monitoring & Troubleshooting Kubernetes
#### Maintaining Kubernetes Clusters
- [ETCD & Containerd](./d6-maintaining-monitoring-troubleshooting-kubernetes/1_maintaining_kubernetes_clusters/1_etcd_containerd/README.md)
- [Cluster Upgrade](./d6-maintaining-monitoring-troubleshooting-kubernetes/1_maintaining_kubernetes_clusters/2_cluster_upgrade/README.md)

#### Logging and Monitoring
- [Logging](./d6-maintaining-monitoring-troubleshooting-kubernetes/2_logging_and_monitoring/1_logging/README.md)
- [JSONPath](./d6-maintaining-monitoring-troubleshooting-kubernetes/2_logging_and_monitoring/2_jsonpath/README.md)
- [Monitoring](./d6-maintaining-monitoring-troubleshooting-kubernetes/2_logging_and_monitoring/3_monitoring/README.md)

#### Troubleshooting
- [Troubleshooting Nodes](./d6-maintaining-monitoring-troubleshooting-kubernetes/3_troublshooting/01_troubleshooting_nodes/README.md)
- [Troubleshooting Control Plane](./d6-maintaining-monitoring-troubleshooting-kubernetes/3_troublshooting/02_troubleshooting_control_plane/README.md)
- [Troubleshooting Workload](./d6-maintaining-monitoring-troubleshooting-kubernetes/3_troublshooting/03_troubleshooting_workload/README.md)

### Domain 7: Kubernetes Security Fundamentals
- [Security Fundamentals](./d7-kubernetes-security-fundamentals/README.md)

### Lab Setup
- [Lab Setup Guide](./lab-setup/README.md)
