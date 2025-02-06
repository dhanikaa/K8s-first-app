# Kubernetes Pod Deployment Guide with Minikube

This guide provides step-by-step instructions on how to set up Minikube and `kubectl` to deploy your first Kubernetes Pod using a `pod.yml` configuration file. It also covers essential `kubectl` commands and how to use `curl` to access the deployed Pod.

## Prerequisites
- macOS (Apple Silicon) with Homebrew installed.
- Docker Desktop (if using the Docker driver).

---

## Installation Instructions

### 1. Installing Minikube
1. **Install Minikube using Homebrew**:
   ```bash
   brew install minikube
   ```
2. **Verify the Installation**:
   ```bash
   minikube version
   ```
3. **Start Minikube**:
   Make sure Docker Desktop is running or use a supported driver:
   ```bash
   minikube start --driver=docker
   ```
   If Docker is unavailable, use:
   ```bash
   minikube start --driver=virtualization
   ```

### 2. Installing `kubectl`
1. **Install `kubectl` using Homebrew**:
   ```bash
   brew install kubectl
   ```
2. **Verify the Installation**:
   ```bash
   kubectl version --client
   ```

---

## Running Your `pod.yml` File
1. **Ensure Minikube Is Running**:
   ```bash
   minikube status
   ```
2. **Deploy the Pod Using `kubectl`**:
   Navigate to the directory containing `pod.yml` and run:
   ```bash
   kubectl apply -f pod.yml
   ```

### `pod.yml` Example
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

---

## Essential `kubectl` Commands

1. **Create a Pod with `kubectl run`**:
   ```bash
   kubectl run nginx --image=nginx
   ```
   - Creates a new Pod named `nginx` using the official Nginx image.
   - This is an alternative to deploying a Pod using a YAML file.

2. **Check Pod Status**:
   ```bash
   kubectl get pods
   ```

3. **List All Pods in Wide Format**:
   ```bash
   kubectl get pods -o wide
   ```

4. **Get Detailed Information About a Pod**:
   ```bash
   kubectl describe pod nginx
   ```
   - Provides detailed information about the `nginx` Pod, including events, conditions, and container states.

5. **View Pod Logs**:
   ```bash
   kubectl logs nginx
   ```

6. **Delete a Pod**:
   ```bash
   kubectl delete pod nginx
   ```

7. **Access the Pod’s Terminal**:
   ```bash
   kubectl exec -it nginx -- /bin/bash
   ```

8. **Get Cluster Information**:
   ```bash
   kubectl cluster-info
   ```

9. **List All Nodes in the Cluster**:
   ```bash
   kubectl get nodes
   ```
   - Displays all nodes in your Kubernetes cluster.

10. **Get Detailed Node Information**:
    ```bash
    kubectl get nodes -o wide
    ```
    - Provides additional details such as internal and external IPs and Kubernetes versions.

---

## Accessing Minikube’s Dashboard
```bash
minikube dashboard
```
This will open the Minikube dashboard in your default browser.

---

## Using `curl` to Access the Pod

### 1. Get Minikube IP Address
```bash
minikube ip
```

### 2. Expose the Pod Using a Service
```bash
kubectl expose pod nginx --type=NodePort --port=80
```

### 3. Get the NodePort
```bash
kubectl get service nginx
```

### 4. Use `curl` to Access the Pod
```bash
curl http://<Minikube_IP>:<NodePort>
```
- Replace `<Minikube_IP>` with the IP from `minikube ip`.
- Replace `<NodePort>` with the port from `kubectl get service nginx`.

### Example
If `minikube ip` returns `192.168.49.2` and the NodePort is `30080`:
```bash
curl http://192.168.49.2:30080
```

---

## Troubleshooting
- **Minikube Not Starting**: Ensure Docker Desktop is running or use a different driver.
- **Pod Not Running**: Check pod status and logs using `kubectl`.

Feel free to contribute or raise issues if you encounter any problems!
