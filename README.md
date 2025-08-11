

# Deploying Nginx on Kubernetes using Minikube

This project demonstrates how to deploy an **Nginx web server** in Kubernetes using Minikube.  
We cover both **Imperative (CLI)** and **Declarative (YAML)** approaches.

---

## 📌 Prerequisites
- [Minikube](https://minikube.sigs.k8s.io/docs/start/) installed
- [kubectl](https://kubernetes.io/docs/tasks/tools/) configured
- Docker installed (for Minikube Docker driver)

---

## 🚀 Approach 1: Imperative (Commands Only)

1. **Start Minikube**
   ```bash
   minikube start
````

2. **Create Deployment**

   ```bash
   minikube kubectl -- create deployment nginx-deployment --image=nginx
   ```

3. **Expose Deployment as a Service**

   ```bash
   minikube kubectl -- expose deployment nginx-deployment --type=NodePort --port=80
   ```

4. **Check Pods**

   ```bash
   minikube kubectl -- get pods
   ```

5. **Get Service URL**

   ```bash
   minikube service nginx-deployment --url
   ```

OUTPUT :

   ```
   http://127.0.0.1:62327/
   ```

6. **Access the Application**
   Open the given URL in your browser and you should see:

   ```
   Welcome to nginx!
   ```

---

## 📜 Approach 2: Declarative (Using YAML Files) #EXAMPLE

**nginx-deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
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

**nginx-service.yaml**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30007
```

**Apply the YAML manifests:**

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

**Get Service URL with Minikube:**

```bash
minikube service nginx-service --url
```
---

## 📷 Output Screenshot

UPLOADED IN TASK5

---

## 📚 Learnings

* **Imperative way** is quick for testing.
* **Declarative way** is preferred for production and version control.
* Minikube is great for local Kubernetes testing.

```
