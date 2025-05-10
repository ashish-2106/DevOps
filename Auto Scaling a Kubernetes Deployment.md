# 📈 Auto Scaling a Kubernetes Deployment using HPA (Minikube)

This experiment demonstrates how to enable **auto-scaling** in a Kubernetes cluster using the **Horizontal Pod Autoscaler (HPA)** based on CPU utilization.

---

## 🎯 Objective

- Deploy a sample app to Kubernetes.
- Configure a Horizontal Pod Autoscaler (HPA).
- Simulate load to trigger auto-scaling.

---

## ⚙️ Prerequisites

Ensure the following tools are installed and configured:

- ✅ Minikube (with metrics-server enabled)
- ✅ kubectl
- ✅ Docker

---

## 🚀 Steps to Execute

### 1. Start Minikube with Metrics Server

```bash
minikube start --cpus=4 --memory=8192
minikube addons enable metrics-server
```

---

### 2. Create Deployment with Resource Limits

Create a file named `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: php-apache
spec:
  replicas: 1
  selector:
    matchLabels:
      app: php-apache
  template:
    metadata:
      labels:
        app: php-apache
    spec:
      containers:
      - name: php-apache
        image: k8s.gcr.io/hpa-example
        resources:
          requests:
            cpu: 200m
          limits:
            cpu: 500m
        ports:
        - containerPort: 80
```

Apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

---

### 3. Expose Deployment as a Service

```bash
kubectl expose deployment php-apache --type=ClusterIP --port=80
```

---

### 4. Create the HPA

```bash
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=5
```

Verify HPA:

```bash
kubectl get hpa
```

---

### 5. Simulate CPU Load

Open a new terminal and run a load generator:

```bash
kubectl run -i --tty load-generator --rm --image=busybox /bin/sh
```

Inside the container, run:

```sh
while true; do wget -q -O- http://php-apache; done
```

This will generate traffic and CPU load.

---

### 6. Monitor Scaling

In another terminal:

```bash
kubectl get hpa -w
```

You should see the number of pods increase automatically as CPU usage goes up.

---

### 7. Stop Load Generator

Use `Ctrl+C` in the load-generator terminal to stop the load.

---

## 🧹 Cleanup

```bash
kubectl delete deployment php-apache
kubectl delete svc php-apache
kubectl delete hpa php-apache
minikube stop
```

---

## 📝 Notes

- HPA requires **metrics-server** to be running.
- Ensure resource limits (`cpu`) are defined in your deployment YAML.
- HPA will not scale up if CPU load is not detected.
