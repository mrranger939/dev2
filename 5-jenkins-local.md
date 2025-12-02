Perfect — below is the **COMPLETE, CLEAN, STEP-BY-STEP CI/CD SETUP USING:**

✅ Local Git Repository
✅ Jenkinsfile inside repo (NOT UI script)
✅ Jenkins Pipeline Job
✅ Poll SCM (auto build on commit)
✅ All permission fixes
✅ **Final working `JAVA_OPTS` fix for local checkout**
✅ All **terminal commands + Jenkins UI steps**

This is your **FINAL EXAM-READY MASTER GUIDE** ✅
You can **follow this exactly from top to bottom on any Linux system**.

---

# ✅ PART 1 — INSTALL REQUIRED TOOLS

```bash
sudo apt update
sudo apt install -y git openjdk-21-jdk jenkins
```

Start Jenkins:

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Open Jenkins in browser:

```
http://localhost:8080
```

Get initial password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# ✅ PART 2 — CREATE LOCAL GIT REPOSITORY

```bash
mkdir -p /home/nawaz/Documents/test/local-cicd-repo
cd /home/nawaz/Documents/test/local-cicd-repo
git init
```

Create sample app:

```bash
echo "print('Hello from CI/CD')" > app.py
```

First commit:

```bash
git add .
git commit -m "Initial commit"
```

---

# ✅ PART 3 — CREATE JENKINSFILE (INSIDE THE REPO)

```bash
nano Jenkinsfile
```

Paste this **FULL CI/CD PIPELINE**:

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo "Source code checked out"
            }
        }

        stage('Build') {
            steps {
                echo "Building application..."
                sh 'python3 app.py'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'echo Tests passed!'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying..."
                sh '''
                mkdir -p /tmp/local-cicd-deploy
                cp app.py /tmp/local-cicd-deploy/
                echo "Deployed to /tmp/local-cicd-deploy"
                '''
            }
        }
    }
}
```

Save & exit.

Commit Jenkinsfile:

```bash
git add Jenkinsfile
git commit -m "Added Jenkinsfile CI/CD pipeline"
```

---

# ✅ PART 4 — FIX PERMISSIONS (MOST IMPORTANT PART YOU FACED)

### ✅ Give Jenkins Ownership (THIS is what solved your issue)

```bash
sudo chown -R jenkins:jenkins /home/nawaz/Documents/test/local-cicd-repo
```

(Optional but safe):

```bash
sudo chmod -R 755 /home/nawaz/Documents/test/local-cicd-repo
```

---

# ✅ PART 5 — ALLOW JENKINS TO CHECKOUT LOCAL REPOSITORIES (FINAL FIX)

Open Jenkins systemd override:

```bash
sudo systemctl edit jenkins
```

Paste **EXACTLY THIS**:

```
[Service]
Environment="JAVA_OPTS=-Dhudson.plugins.git.GitSCM.ALLOW_LOCAL_CHECKOUT=true"
```

Save & exit.

Restart Jenkins properly:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart jenkins
```

Verify flag is applied:

```bash
ps aux | grep jenkins
```

✅ You MUST see:

```
-Dhudson.plugins.git.GitSCM.ALLOW_LOCAL_CHECKOUT=true
```

---

# ✅ PART 6 — CREATE JENKINS PIPELINE JOB (UI STEPS)

1. Open Jenkins → **New Item**
2. Name:

```
local-cicd-pipeline
```

3. Select ✅ **Pipeline**
4. Click **OK**

---

# ✅ PART 7 — CONNECT LOCAL GIT REPO TO PIPELINE

Inside job configuration → **Pipeline section**:

| Field          | Value                                        |
| -------------- | -------------------------------------------- |
| Definition     | `Pipeline script from SCM`                   |
| SCM            | `Git`                                        |
| Repository URL | `/home/nawaz/Documents/test/local-cicd-repo` |
| Branch         | `*/master`                                   |
| Script Path    | `Jenkinsfile`                                |

---

# ✅ PART 8 — ENABLE AUTO BUILD ON LOCAL COMMITS (POLL SCM)

Scroll to **Build Triggers**:

✅ Check **Poll SCM**

Schedule:

```
* * * * *
```

✅ Save the job.

---

# ✅ PART 9 — FIRST BUILD (MANUAL)

Click:

👉 **Build Now**

Console output will show:

```
Building application...
Hello from CI/CD
Running tests...
Deploying...
Deployed to /tmp/local-cicd-deploy
Finished: SUCCESS
```

---

# ✅ PART 10 — TEST AUTO BUILD (LOCAL COMMIT TRIGGER)

Change app:

```bash
echo "print('Version 2 - Auto Trigger')" > app.py
git add .
git commit -m "Trigger auto build"
```

⏳ Wait 1 minute…

✅ Jenkins will **automatically build**
✅ Build → Test → Deploy runs again ✅

Verify deployment:

```bash
ls /tmp/local-cicd-deploy
cat /tmp/local-cicd-deploy/app.py
```

---

# ✅ FINAL ARCHITECTURE (EXAM ANSWER)

```
Local Git Repo
      ↓
Poll SCM (Every 1 min)
      ↓
Jenkins Pipeline
      ↓
Checkout → Build → Test → Deploy
      ↓
/tmp/local-cicd-deploy
```

---

# ✅ FINAL EXAM EXPLANATION (YOU CAN WRITE THIS)

> “I created a local Git repository and added a Jenkinsfile inside it. I configured a Jenkins Pipeline job using ‘Pipeline script from SCM’ and enabled Poll SCM so Jenkins automatically checks for new commits. Whenever a commit is made, Jenkins automatically executes the CI/CD pipeline including build, test, and deployment stages. Since Jenkins blocks local Git paths by default, I enabled local checkout using the JAVA_OPTS systemd override.”

---

# ✅ EVERYTHING YOU IMPLEMENTED

✅ Git Local Repo
✅ Jenkinsfile inside repo
✅ Jenkins Pipeline Job
✅ Poll SCM Auto Trigger
✅ Build Stage
✅ Test Stage
✅ Deploy Stage
✅ Ownership Fix (`chown`)
✅ Jenkins Local Checkout Fix (`JAVA_OPTS`)
✅ Fully Automatic Offline CI/CD

---

If you want, I can now also:

✅ Convert this into your **lab record format**
✅ Add **slave node execution**
✅ Add **real unit testing**
✅ Add **web app deployment**

Just tell me ✅

