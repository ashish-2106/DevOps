# ⚙️ CI/CD Pipeline with Jenkins on Kubernetes (Minikube)

This experiment demonstrates how to set up a CI/CD pipeline using **Jenkins** deployed on a **Kubernetes cluster via Minikube**.

---

## 🧾 Objective

- Deploy Jenkins using Helm on Minikube.
- Configure Jenkins to automate build, test, and deployment using a `Jenkinsfile` from your GitHub repository.

---

## 📋 Prerequisites

Make sure the following tools are installed and configured:

- ✅ Minikube (started with enough resources)
- ✅ kubectl
- ✅ Helm
- ✅ Docker
- ✅ A GitHub repository with a sample app (e.g., Node.js or Java)

---

## 🚀 Steps to Execute

### 1. Start Minikube

```bash
minikube start --cpus=4 --memory=8192
```

### 2. Add and Update Jenkins Helm Repo

```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
```

### 3. Install Jenkins on Minikube

```bash
helm install jenkins jenkins/jenkins --namespace jenkins --create-namespace
```

> ⏳ Wait a few minutes for the Jenkins pod to be ready.

---

### 4. Access Jenkins Dashboard

#### a. Get Jenkins admin password:

```bash
kubectl get pods -n jenkins
kubectl exec --namespace jenkins -it <jenkins-pod-name> -- cat /var/jenkins_home/secrets/initialAdminPassword
```

#### b. Port-forward Jenkins service:

```bash
kubectl port-forward svc/jenkins 8080:8080 -n jenkins
```

Then go to [http://localhost:8080](http://localhost:8080) in your browser.

---

### 5. Configure Jenkins UI

- Enter the admin password.
- Install **Suggested Plugins**.
- Create an **admin user**.
- Go to **New Item > Pipeline**.
- Connect to your GitHub repo and add a `Jenkinsfile`.

---

### 6. Sample Jenkinsfile

Create a `Jenkinsfile` in the root of your repository:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

> ✅ Jenkins will automatically detect this file and run the pipeline on each push (if configured with GitHub Webhooks or SCM Polling).

---

## 🧹 Cleanup

To delete everything:

```bash
helm uninstall jenkins -n jenkins
kubectl delete namespace jenkins
minikube stop
```

---

## 📌 Notes

- Minikube doesn't support `LoadBalancer` services natively. Jenkins is accessed via `kubectl port-forward`.
- You can expose Jenkins using an Ingress controller if preferred.
