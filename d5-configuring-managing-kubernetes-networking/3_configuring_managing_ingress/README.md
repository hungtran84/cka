# Lab: Deploying an Ingress Controller and Configuring Ingress Rules in Kubernetes

## Objectives
- Deploy and configure an NGINX Ingress Controller.
- Expose services using Ingress with various configurations.
- Implement path-based and name-based routing for multiple services.
- Secure Ingress traffic using TLS.
- Clean up resources after the lab.

---

## Steps

### 1. Deploying the Ingress Controller
1. Apply the NGINX Ingress Controller manifest:
   ```bash
   kubectl apply -f deploy.yaml
   ```
2. Verify the Ingress Controller is online:
   ```bash
   kubectl get pods --namespace ingress-nginx
   ```
3. Check the status of the `LoadBalancer` service:
   ```bash
   kubectl get services --namespace ingress-nginx
   ```
4. Set the default Ingress class (optional):
   ```bash
   kubectl describe ingressclasses nginx
   kubectl annotate ingressclasses nginx "ingressclass.kubernetes.io/is-default-class=true"
   ```

---

### 2. Single Service with Ingress
1. Create a deployment, scale it, and expose it as a `ClusterIP` service:
   ```bash
   kubectl create deployment hello-world-service-single --image=ghcr.io/hungtran84/hello-app:1.0
   kubectl scale deployment hello-world-service-single --replicas=2
   kubectl expose deployment hello-world-service-single --port=80 --target-port=8080 --type=ClusterIP
   ```
2. Create an Ingress routing to the single service:
   ```bash
   kubectl apply -f ingress-single.yaml
   ```
3. Check the status of the Ingress and the associated service:
   ```bash
   kubectl get ingress
   kubectl describe ingress ingress-single
   ```
4. Access the application via the public IP:
   ```bash
   INGRESSIP=$(kubectl get ingress -o jsonpath='{ .items[].status.loadBalancer.ingress[].ip }')
   curl http://$INGRESSIP
   ```

---

### 3. Multiple Services with Path-Based Routing
1. Create two additional deployments and expose them as services:
   ```bash
   kubectl create deployment hello-world-service-blue --image=ghcr.io/hungtran84/hello-app:1.0
   kubectl create deployment hello-world-service-red  --image=ghcr.io/hungtran84/hello-app:1.0
   kubectl expose deployment hello-world-service-blue --port=4343 --target-port=8080 --type=ClusterIP
   kubectl expose deployment hello-world-service-red  --port=4242 --target-port=8080 --type=ClusterIP
   ```
2. Create an Ingress with path-based routing:
   ```bash
   kubectl apply -f ingress-path.yaml
   ```
3. Check the status and routing rules:
   ```bash
   kubectl get ingress --watch
   kubectl describe ingress ingress-path
   ```
4. Access services using paths and host headers:
   ```bash
   curl http://$INGRESSIP/
   curl http://$INGRESSIP/red  --header 'Host: path.example.com'
   curl http://$INGRESSIP/blue --header 'Host: path.example.com'
   ```

---

### 4. Name-Based Virtual Hosts
1. Create an Ingress for name-based virtual hosts:
   ```bash
   kubectl apply -f ingress-namebased.yaml
   kubectl get ingress --watch
   ```
2. Access services using host headers:
   ```bash
   curl http://$INGRESSIP/ --header 'Host: red.example.com'
   curl http://$INGRESSIP/ --header 'Host: blue.example.com'
   ```

---

### 5. TLS Example
1. Generate a certificate:
   ```bash
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
       -keyout tls.key -out tls.crt -subj "/C=VN/ST=HN/L=TNR/O=IT/OU=IT/CN=training.hungtran.net"
   ```
2. Create a TLS secret:
   ```bash
   kubectl create secret tls tls-secret --key tls.key --cert tls.crt
   ```
3. Apply an Ingress using the certificate:
   ```bash
   kubectl apply -f ingress-tls.yaml
   kubectl get ingress --watch
   ```
4. Test HTTPS access:
   ```bash
   curl https://training.hungtran.net:443 --resolve training.hungtran.net:443:$INGRESSIP --insecure --verbose
   ```

---

### 6. Cleanup
1. Delete resources created during the lab:
   ```bash
   kubectl delete ingresses ingress-path
   kubectl delete ingresses ingress-tls
   kubectl delete ingresses ingress-namebased
   kubectl delete deployment hello-world-service-single
   kubectl delete deployment hello-world-service-red
   kubectl delete deployment hello-world-service-blue
   kubectl delete service hello-world-service-single
   kubectl delete service hello-world-service-red
   kubectl delete service hello-world-service-blue
   kubectl delete secret tls-secret
   rm tls.crt
   rm tls.key
   ```
2. Delete the Ingress Controller and its resources:
   ```bash
   kubectl delete -f deploy.yaml
   ```

---

## Summary
In this lab, you:
- Deployed and configured an NGINX Ingress Controller.
- Exposed single and multiple services using Ingress.
- Implemented path-based and name-based routing.
- Secured traffic using TLS certificates.
- Cleaned up all resources to maintain a clean environment.
