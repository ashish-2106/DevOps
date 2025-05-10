# 🌐 Creating a Kubernetes Load Balancer (Minikube + MetalLB)

This experiment demonstrates how to expose a Kubernetes deployment using a **LoadBalancer service** in a local Minikube cluster with **MetalLB**.

---

## 📌 Objective

- Deploy a sample NGINX app on Kubernetes.
- Use MetalLB to expose the app via a LoadBalancer IP (simulated in Minikube).

---

## ⚙️ Prerequisites

Ensure the following tools are installed and configured:

- ✅ Minikube
- ✅ kubectl
- ✅ MetalLB installed on Minikube (to simulate LoadBalancer)
- ✅ A sample app (like NGINX)

---

## 🚀 Steps to Execute

### 1. Start Minikube

```bash
minikube start --cpus=4 --memory=8192
```

---

### 2. Enable MetalLB

```bash
minikube addons enable metallb
```

Now configure an IP address pool for MetalLB.

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.10/config/manifests/metallb-native.yaml
```

Create a `metallb-config.yaml`:

```yaml
# metallb-config.yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: my-ip-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.49.240-192.168.49.250  # adjust based on your Minikube IP range

---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: l2
  namespace: metallb-system
```

Apply the configuration:

```bash
kubectl apply -f metallb-config.yaml
```

---

### 3. Deploy Sample NGINX App

Create a file `deployment.yaml`:

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

---

### 4. Create LoadBalancer Service

Create a file `service.yaml`:

```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

Apply the service:

```bash
kubectl apply -f service.yaml
```

---

### 5. Access the Application

Get the external IP assigned by MetalLB:

```bash
kubectl get svc nginx-service
```

Example output:

```
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
nginx-service   LoadBalancer   10.96.148.46   192.168.49.240   80:30693/TCP   2m
```

Open the app in your browser:

```
http://<EXTERNAL-IP>
```

---

## 🧹 Cleanup

To clean up all resources:

```bash
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
kubectl delete -f metallb-config.yaml
minikube stop
```
