Perfect 👍 Let’s go step-by-step through your **DevOps Lab Experiment**:
You’ll build, containerize, and run a simple **Flask web app** using **Docker** on **Linux**.

---

## 🧩 **Goal**

Build and deploy a simple Flask app using Docker, with a clear understanding of Dockerfile instructions.

---

## 🖥️ **Requirements**

Make sure you have:

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Verify installation:

```bash
docker --version
```

---

## 🪶 **Step 1: Create Project Folder**

```bash
mkdir flask-docker-app
cd flask-docker-app
```

---

## 🧠 **Step 2: Create a Simple Flask App**

Create a file `app.py`:

```bash
nano app.py
```

Paste this:

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, Docker! This is a Flask app running in a container."

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

## 📄 **Step 3: Create Requirements File**

Create a `requirements.txt` file:

```bash
nano requirements.txt
```

Add:

```
flask
```

---

## 🐳 **Step 4: Create Dockerfile**

Create a file named `Dockerfile` (no extension):

```bash
nano Dockerfile
```

Paste this:

```dockerfile
FROM python:3.12-slim

# Copy only the requirements first (better for caching)
COPY requirements.txt /app/

WORKDIR /app

# Install all dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Then copy the rest of the application code
COPY . /app

EXPOSE 5000
CMD ["python", "app.py"]

```
or

```dockerfile

FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]

```

---

## 🧾 **Step 5: Explanation of Each Dockerfile Instruction**

| Instruction | Purpose                                                                           |
| ----------- | --------------------------------------------------------------------------------- |
| **FROM**    | Specifies the base image (here: `python:3.12-slim`) used to build your image.     |
| **RUN**     | Executes commands in a new layer of the image (e.g., installing dependencies).    |
| **COPY**    | Copies files/directories from the host machine into the container’s filesystem.   |
| **EXPOSE**  | Documents the port that the container will listen on (5000 for Flask).            |
| **CMD**     | Defines the default command that runs when the container starts (runs Flask app). |

---

## ⚙️ **Step 6: Build the Docker Image**

Run this command inside the folder:

```bash
docker build -t flask-docker-app .
```

Explanation:

* `-t` assigns a tag (name) to your image.
* `.` means current directory (where the Dockerfile is).

Check if image was created:

```bash
docker images
```

---

## 🚀 **Step 7: Run the Container**

Run your image in a container:

```bash
docker run -d -p 5000:5000 flask-docker-app
```

Explanation:

* `-d` runs it in detached mode (background).
* `-p 5000:5000` maps container port 5000 to your local port 5000.

Check running containers:

```bash
docker ps
```

---

## 🌐 **Step 8: Test the App**

Open a browser or use curl:

```bash
curl http://localhost:5000
```

or go to
👉 `http://localhost:5000`

You should see:

```
Hello, Docker! This is a Flask app running in a container.
```

---

## 🧹 **Step 9: Stop and Remove the Container**

```bash
docker stop <container_id>
docker rm <container_id>
```

---

## 🧰 **Optional Verification Commands**

| Command                         | Description                                 |
| ------------------------------- | ------------------------------------------- |
| `docker images`                 | Lists all images on your system             |
| `docker ps -a`                  | Lists all containers (running and stopped)  |
| `docker inspect <container_id>` | Shows detailed info about a container       |
| `docker logs <container_id>`    | Shows container logs (useful for debugging) |

---

## ✅ **Final Output**

* Flask app runs successfully in a Docker container.
* Accessible at `http://localhost:5000`
* Verified container creation and image build.

---

Would you like me to give a **Node.js version** of the same lab (for comparison)?
