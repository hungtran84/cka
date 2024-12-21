```markdown
# Lab: Exposing and Accessing Applications with Services on Kubernetes

## Objectives
- Understand the different types of Kubernetes Services (`ClusterIP`, `NodePort`, `LoadBalancer`, `ExternalName`).
- Learn how to expose deployments using Services.
- Access applications within the cluster and from external clients.
- Scale deployments and observe how services handle load balancing.

---

## Steps

### ClusterIP Service

1. **Create a Deployment**  
   ```bash
   kubectl create deployment hello-world-clusterip \
       --image=ghcr.io/hungtran84/hello-app:1.0
   ```

2. **Expose the Deployment as a ClusterIP Service**  
   ```bash
   kubectl expose deployment hello-world-clusterip \
       --port=80 --target-port=8080 --type ClusterIP
   ```

3. **View Service Details**  
   ```bash
   kubectl get service
   ```

4. **Access the Service from Inside the Cluster**  
   ```bash
   SERVICEIP=$(kubectl get service hello-world-clusterip -o jsonpath='{ .spec.clusterIP }')
   echo $SERVICEIP
   kubectl run bb --image=nginx:alpine --restart=Never -it --rm -- curl http://"${SERVICEIP}"
   ```

5. **Inspect Service Endpoints and Pods**  
   ```bash
   kubectl get endpoints hello-world-clusterip
   kubectl get pods -o wide
   ```

6. **Access the Application directly on Pod's Target Port**  
   ```bash
   PODIP=$(kubectl get endpoints hello-world-clusterip -o jsonpath='{ .subsets[].addresses[].ip }')
   echo $PODIP
   curl http://$PODIP:8080
   ```

7. **Scale the Deployment and Observe Load Balancing**  
   ```bash
   kubectl scale deployment hello-world-clusterip --replicas=6
   kubectl get endpoints hello-world-clusterip
   kubectl run bb --image=nginx:alpine --restart=Never -it --rm -- curl http://"${SERVICEIP}"
   ```

8. **Examine Service Selectors and Pod Labels**  
   ```bash
   kubectl describe service hello-world-clusterip
   kubectl get pods --show-labels
   ```

9. **Cleanup**  
   ```bash
   kubectl delete deployments hello-world-clusterip
   kubectl delete service hello-world-clusterip
   ```

---

### NodePort Service

1. **Create a Deployment**  
   ```bash
   kubectl create deployment hello-world-nodeport \
       --image=ghcr.io/hungtran84/hello-app:1.0
   ```

2. **Expose the Deployment as a NodePort Service**  
   ```bash
   kubectl expose deployment hello-world-nodeport \
       --port=80 --target-port=8080 --type NodePort
   ```

3. **Inspect Service Details**  
   ```bash
   kubectl get service
   CLUSTERIP=$(kubectl get service hello-world-nodeport -o jsonpath='{ .spec.clusterIP }')
   PORT=$(kubectl get service hello-world-nodeport -o jsonpath='{ .spec.ports[].port }')
   NODEPORT=$(kubectl get service hello-world-nodeport -o jsonpath='{ .spec.ports[].nodePort }')
   ```

4. **Access the Service via NodePort**  
   ```bash
   curl http://<EXTERNAL-IP>:$NODEPORT
   ```

5. **Access the Service via ClusterIP**  
   ```bash
   kubectl run bb --image=nginx:alpine --restart=Never -it --rm -- curl http://"${CLUSTERIP}:${PORT}"
   ```

6. **Cleanup**  
   ```bash
   kubectl delete service hello-world-nodeport
   kubectl delete deployment hello-world-nodeport
   ```

---

### LoadBalancer Service (GCP)

1. **Create a Deployment**  
   ```bash
   kubectl create deployment hello-world-loadbalancer \
       --image=ghcr.io/hungtran84/hello-app:1.0
   ```

2. **Expose the Deployment as a LoadBalancer Service**  
   ```bash
   kubectl expose deployment hello-world-loadbalancer \
       --port=80 --target-port=8080 --type LoadBalancer
   ```

3. **Wait for External IP Assignment**  
   ```bash
   kubectl get service
   ```

4. **Access the Service via Load Balancer**  
   ```bash
   LOADBALANCERIP=$(kubectl get service hello-world-loadbalancer -o jsonpath='{ .status.loadBalancer.ingress[].ip }')
   curl http://$LOADBALANCERIP
   ```

5. **Inspect LoadBalancer Details**  
   ```bash
   kubectl get service hello-world-loadbalancer
   ```

6. **Cleanup**  
   ```bash
   kubectl delete deployment hello-world-loadbalancer
   kubectl delete service hello-world-loadbalancer
   ```

---

### ExternalName Service

1. **Create an ExternalName Service**  
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: hello-world-api
   spec:
     type: ExternalName
     externalName: hello-world.api.example.com
   ```  
   Save the file as `externalname-service.yaml`.

2. **Apply the YAML Manifest**  
   ```bash
   kubectl apply -f externalname-service.yaml
   ```

3. **Check the Service Details**  
   ```bash
   kubectl get service hello-world-api
   ```

4. **Test the Service**  
   Access the external service from within the cluster using a test pod.  
   ```bash
   kubectl run bb --image=nginx:alpine --restart=Never -it --rm -- curl http://hello-world-api
   ```

5. **Cleanup**  
   ```bash
   kubectl delete service hello-world-api
   ```

---

## Summary

In this lab, you:
- Created and exposed applications using `ClusterIP`, `NodePort`, `LoadBalancer`, and `ExternalName` services.
- Accessed services from within the cluster and externally.
- Observed Kubernetes' ability to load balance traffic and manage scaling.
- Explored the relationship between service types and their endpoints.
```