# Lab: Accessing Information with jsonpath

## Objectives
- Learn how to use `jsonpath` to extract and manipulate data from Kubernetes objects.
- Explore sorting and formatting outputs using `jsonpath`.

---

## Steps

### 1. Create a workload and scale it
```bash
kubectl create deployment hello-world --image=ghcr.io/hungtran84/hello-app:1.0
kubectl scale deployment hello-world --replicas=3
kubectl get pods -l app=hello-world
```

### 2. Access the JSON output of pods
```bash
kubectl get pods -l app=hello-world -o json > pods.json
```

### 3. Display the names of all pods
```bash
kubectl get pods -l app=hello-world -o jsonpath='{ .items[*].metadata.name }'
```

### 4. Display pod names with a newline after each name
```bash
kubectl get pods -l app=hello-world -o jsonpath='{ .items[*].metadata.name }{"\n"}'
```

### 5. Display the first pod's name
```bash
kubectl get pods -l app=hello-world -o jsonpath='{ .items[0].metadata.name }{"\n"}'
```

### 6. Get all container images in use by pods in all namespaces
```bash
kubectl get pods --all-namespaces -o jsonpath='{ .items[*].spec.containers[*].image }{"\n"}'
```

### 7. Filter a specific value in a list
Use filters like `?()` and `@` to extract specific data.
```bash
kubectl get nodes c1-cp1 -o json | more
kubectl get nodes -o jsonpath="{.items[*].status.addresses[?(@.type=='InternalIP')].address}"
```

### 8. Sort pod output by a specific field
```bash
kubectl get pods -A -o jsonpath='{ .items[*].metadata.name }{"\n"}' --sort-by=.metadata.name
```

### 9. Sort pods by creation timestamp and display it
```bash
kubectl get pods -A -o jsonpath='{ .items[*].metadata.name }{"\n"}' \
    --sort-by=.metadata.creationTimestamp \
    --output=custom-columns='NAME:metadata.name,CREATIONTIMESTAMP:metadata.creationTimestamp'
```

### 10. Clean up resources
```bash
kubectl delete deployment hello-world
```

---

## Additional Examples

### Print pod names with a new line for each object
```bash
kubectl get pods -l app=hello-world -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
```

### Combine pod names with container images
```bash
kubectl get pods -l app=hello-world -o jsonpath='{range .items[*]}{.metadata.name}{.spec.containers[*].image}{"\n"}{end}'
```

### List all container images across all namespaces, sorted by name
```bash
kubectl get pods -A -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[*].image}{"\n"}{end}' \
    --sort-by=.spec.containers[*].image
```

### Extract node internal IPs and hostnames
```bash
kubectl get nodes -o jsonpath='{range .items[*]}{.status.addresses[?(@.type=="InternalIP")].address}{"\n"}{end}'
kubectl get nodes -o jsonpath='{range .items[*]}{.status.addresses[?(@.type=="Hostname")].address}{"\n"}{end}'
```

### Sort pods by container image and display pod names with images
```bash
kubectl get pods -A -o jsonpath='{ .items[*].spec.containers[*].image }' --sort-by=.spec.containers[*].image
kubectl get pods -A -o jsonpath='{range .items[*]}{.metadata.name }{"\t"}{.spec.containers[*].image }{"\n"}{end}' --sort-by=.spec.containers[*].image
```

### Add spaces or tabs for better readability
```bash
kubectl get pods -l app=hello-world -o jsonpath='{range .items[*]}{.metadata.name}{" "}{.spec.containers[*].image}{"\n"}{end}'
kubectl get pods -l app=hello-world -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[*].image}{"\n"}{end}'
```

---

## Summary
This lab introduced the basics of `jsonpath` in Kubernetes, demonstrated its use for extracting specific data, and explored advanced formatting and sorting techniques. Clean up resources when finished to maintain a clean environment.
