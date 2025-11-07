

### **STEP 1: Install & Configure Docker**

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable --now docker
```

Add your user to the Docker group (so you can run Docker without `sudo`):

```bash
sudo usermod -aG docker $USER
newgrp docker    # refresh group
```

Verify Docker works:

```bash
docker run hello-world
```

---

### ☸️ **STEP 2: Install Minikube (Local Kubernetes)**

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Check installation:

```bash
minikube version
```

---

### 🚀 **STEP 3: Start Minikube Cluster**

Use Docker as the driver (works best with your setup):

```bash
minikube start --driver=docker
```

Verify cluster nodes:

```bash
minikube kubectl -- get nodes
```

(Optional) View all resources:

```bash
minikube kubectl -- get all --all-namespaces
```

---

### 🧠 **STEP 4: Create Flask Application**

```bash
mkdir flask-app
cd flask-app
```

Create `app.py`:

```bash
nano app.py
```

Paste:

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Flask on Minikube!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

### 🐳 **STEP 5: Create Dockerfile**

```bash
nano Dockerfile
```

Paste:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY app.py /app
RUN pip install flask
EXPOSE 5000
CMD ["python", "app.py"]
```

---

### ⚙️ **STEP 6: Use Minikube’s Docker Environment**

This ensures the image builds **inside** Minikube’s Docker (so Kubernetes can use it).

```bash
eval $(minikube docker-env)
```

Now build the image:

```bash
docker build -t flask-app:1.0 .
```

Verify image exists:

```bash
docker images
```

---

### 🧱 **STEP 7: Create Deployment**

Create and manage Pods with this command:

```bash
minikube kubectl -- create deployment flask-app --image=flask-app:1.0
```

Verify deployment and pods:

```bash
minikube kubectl -- get deployments
minikube kubectl -- get pods
```

---

### 🌐 **STEP 8: Expose the Deployment as a Service**

Expose the Flask app to outside network using NodePort:

```bash
minikube kubectl -- expose deployment flask-app --type=NodePort --port=5000
```

Check service:

```bash
minikube kubectl -- get services
```

---

### 🌍 **STEP 9: Access the Flask App**

Get the URL to access your app:

```bash
minikube service flask-app --url
```

Example output:

```
http://192.168.49.2:32000
```

Open that URL in your browser — you’ll see:

```
Hello from Flask on Minikube!
```

---

### 🧹 **STEP 10: Cleanup (Optional)**

```bash
minikube kubectl -- delete service flask-app
minikube kubectl -- delete deployment flask-app
minikube stop
```

---

## 🧾 **Everything You Just Demonstrated**

| Concept                   | What You Showed                                      |
| ------------------------- | ---------------------------------------------------- |
| **Docker**                | Containerized the Flask app                          |
| **Kubernetes (Minikube)** | Managed and deployed container                       |
| **Pod**                   | Single running instance of your Flask container      |
| **Deployment**            | Controlled Pod creation and scaling                  |
| **Service (NodePort)**    | Exposed app to external users                        |
| **kubectl orchestration** | Verified cluster, scaled, and accessed pods/services |

---

✅ **Final Output Expected**

* Flask app deployed successfully inside Kubernetes Pod
* Accessible via URL from `minikube service flask-app --url`
* You can scale it easily using:

  ```bash
  minikube kubectl -- scale deployment flask-app --replicas=3
  minikube kubectl -- get pods
  ```

---

Yes Nawaz — these exact commands **will work perfectly** on your Linux system for the DevOps lab.
Would you like me to give you a **one-click shell script** that runs all these steps automatically (for lab submission or demo)?
