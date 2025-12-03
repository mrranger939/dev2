Perfect — here is the **FULL, SINGLE-MESSAGE, EXAM-READY GUIDE** for the **same Kubernetes + Docker + Minikube workflow**, but using a **Node.js + Express app instead of Flask** ✅
This is written **exactly like your Flask version**, just switched to **Node.js (Express)**.

You can **paste this directly into your lab record**.

---

# ✅ **DevOps Lab: Docker + Kubernetes Using Node.js (Express) with Minikube**

---

## ✅ **1) Install & Configure Docker**

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable --now docker

sudo usermod -aG docker $USER
newgrp docker

docker run hello-world
```

---

## ✅ **2) Install Minikube**

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube version
```

---

## ✅ **3) Start Kubernetes Cluster**

```bash
minikube start --driver=docker
```

Verify:

```bash
minikube kubectl -- get nodes
minikube kubectl -- get all --all-namespaces
```

---

## ✅ **4) Create Node.js (Express) Application**

```bash
mkdir node-app
cd node-app
```

Initialize project:

```bash
npm init -y
npm install express
```

Create `index.js`:

```bash
nano index.js
```

### ✅ `index.js`

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from Node.js Express on Minikube!");
});

app.listen(5000, () => {
  console.log("Server running on port 5000");
});
```

---

## ✅ **5) Create Dockerfile**

```bash
nano Dockerfile
```

### ✅ `Dockerfile`

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json /app
RUN npm install
COPY . /app
EXPOSE 5000
CMD ["node", "index.js"]
```

---

## ✅ **6) Build Image Inside Minikube**

```bash
eval $(minikube docker-env)

docker build -t node-app:1.0 .
docker images
```

---

## ✅ **7) Create Kubernetes YAML Files**

```bash
mkdir k8s
cd k8s
nano deployment.yaml
```

---

### ✅ **Minimal `deployment.yaml` (Node.js – Exam Perfect)**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node
        image: node-app:1.0
        ports:
        - containerPort: 5000
```

---

### ✅ **`service.yaml`**

```bash
nano service.yaml
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-service
spec:
  type: NodePort
  selector:
    app: node-app
  ports:
    - port: 5000
      targetPort: 5000
```

---

## ✅ **8) Apply YAMLs to the Cluster**

```bash
minikube kubectl -- apply -f k8s/deployment.yaml
minikube kubectl -- apply -f k8s/service.yaml
```

---

## ✅ **9) Verify Resources**

```bash
minikube kubectl -- get deployments
minikube kubectl -- get pods
minikube kubectl -- get svc
```

---

## ✅ **10) Access the Application**

```bash
minikube service node-service --url
```

OR:

```bash
curl $(minikube service node-service --url)
```

✅ Output in browser / curl:

```
Hello from Node.js Express on Minikube!
```

---

## ✅ **11) Debug & Logs**

```bash
minikube kubectl -- get pods
minikube kubectl -- describe pod <pod-name>
minikube kubectl -- logs <pod-name>
minikube kubectl -- rollout status deployment/node-deployment
```

---

## ✅ **12) Scaling the Application**

```bash
minikube kubectl -- scale deployment/node-deployment --replicas=5
minikube kubectl -- get pods
```

---

## ✅ **13) Rolling Update**

```bash
docker build -t node-app:1.1 .

minikube kubectl -- set image deployment/node-deployment node=node-app:1.1 --record
minikube kubectl -- rollout status deployment/node-deployment
minikube kubectl -- rollout history deployment/node-deployment
```

---

## ✅ **14) Rollback**

```bash
minikube kubectl -- rollout undo deployment/node-deployment
```

---

## ✅ **15) Cleanup**

```bash
minikube kubectl -- delete -f k8s/service.yaml
minikube kubectl -- delete -f k8s/deployment.yaml
minikube stop
eval $(minikube docker-env -u)
```

---

## ✅ **Project Folder Structure**

```
node-app/
  index.js
  package.json
  Dockerfile
  k8s/
    deployment.yaml
    service.yaml
```

---

# ✅ **Exam Theory (Node.js Version)**

### ✅ What You Declared

* A **Deployment** running 3 replicas of Node.js Express app
* A **NodePort Service** exposing the app externally

### ✅ How Kubernetes Manages Orchestration

* **Scheduler** assigns Pods to nodes
* **Kubelet** runs Node.js containers
* **Deployment Controller** maintains replicas
* **Service** load-balances traffic
* **Self-Healing**: failed Pods are automatically recreated

### ✅ Scaling

```bash
kubectl scale deployment node-deployment --replicas=5
```

### ✅ Declarative Advantage

YAML files define the desired state; Kubernetes continuously maintains it.

---

✅ This is now the **Node.js + Express equivalent of your Flask Kubernetes experiment**
✅ Fully **exam-ready**
✅ Uses **Minikube only**
✅ No kubectl installation needed
✅ Supports **scaling + rolling update + rollback**

---

If you want, I can also give you:
✅ **1-page Node.js Kubernetes lab report**
✅ **Viva Q&A for Docker + Kubernetes**
✅ **Puppet + Kubernetes combined viva**

Tell me what you want next ✅
