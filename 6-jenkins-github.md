Perfect — here is the **COMPLETE, CLEAN, FINAL STEP-BY-STEP GUIDE** for:

✅ **Public GitHub Repo**
✅ **Only 2 files:** `app.py` and `test.py`
✅ **Jenkinsfile stored in the repo**
✅ **Jenkins Pipeline from SCM**
✅ **Auto-build using Poll SCM (NO webhooks, NO ngrok)**
✅ **Build → Test → Deploy**
✅ **All terminal commands + all Jenkins UI steps**

You can **follow this exactly from top to bottom**.

---

# ✅ PART 1 — CREATE A PUBLIC GITHUB REPO

On GitHub:

1. Click **New Repository**
2. Repo name:

   ```
   jenkins-github-cicd
   ```
3. Set:

   * ✅ Public
   * ❌ Do NOT add README
4. Click **Create**

You will get a URL like:

```
https://github.com/<your-username>/jenkins-github-cicd.git
```

---

# ✅ PART 2 — CLONE THE REPO LOCALLY

```bash
cd ~/Documents/test
git clone https://github.com/<your-username>/jenkins-github-cicd.git
cd jenkins-github-cicd
```

---

# ✅ PART 3 — CREATE app.py and test.py (ONLY 2 FILES)

### ✅ app.py

```bash
nano app.py
```

Paste:

```python
print("Hello from GitHub Jenkins CI/CD")
```

Save & exit.

---

### ✅ test.py

```bash
nano test.py
```

Paste:

```python
print("Tests executed successfully")
```

Save & exit.

---

# ✅ PART 4 — CREATE JENKINSFILE (PIPELINE FILE)

```bash
nano Jenkinsfile
```

Paste this **FULL WORKING PIPELINE**:

```groovy
pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo "Running application..."
                sh 'python3 app.py'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'python3 test.py'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh '''
                mkdir -p /tmp/github-cicd-deploy
                cp app.py test.py /tmp/github-cicd-deploy/
                echo "Deployment completed to /tmp/github-cicd-deploy"
                '''
            }
        }
    }
}
```

Save & exit.

---

# ✅ PART 5 — PUSH EVERYTHING TO GITHUB

```bash
git add .
git commit -m "Added app.py, test.py and Jenkins CI/CD pipeline"
git push origin main
```

(If your branch is `master`, use `master` instead of `main`.)

---

# ✅ PART 6 — CREATE JENKINS PIPELINE JOB (UI STEPS)

1. Open Jenkins:

```
http://localhost:8080
```

2. Click ✅ **New Item**
3. Job name:

```
github-cicd-pipeline
```

4. Select ✅ **Pipeline**
5. Click ✅ **OK**

---

# ✅ PART 7 — CONNECT GITHUB REPO TO JENKINS

In **Pipeline section**:

| Field          | Value                                                        |
| -------------- | ------------------------------------------------------------ |
| Definition     | `Pipeline script from SCM`                                   |
| SCM            | `Git`                                                        |
| Repository URL | `https://github.com/<your-username>/jenkins-github-cicd.git` |
| Credentials    | Leave EMPTY (repo is public)                                 |
| Branch         | `*/main`                                                     |
| Script Path    | `Jenkinsfile`                                                |

---

# ✅ PART 8 — ENABLE AUTO BUILD (POLL SCM)

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

Console Output will show:

```
Running application...
Hello from GitHub Jenkins CI/CD
Running tests...
Tests executed successfully
Deploying application...
Deployment completed to /tmp/github-cicd-deploy
Finished: SUCCESS
```

✅ CI/CD is working.

---

# ✅ PART 10 — TEST AUTO BUILD FROM GITHUB PUSH

Now modify app.py:

```bash
echo "print('Version 2 from Auto CI/CD')" > app.py
git add app.py
git commit -m "Auto CI/CD test"
git push origin main
```

⏳ Wait 1 minute…

✅ Jenkins will **automatically trigger build**
✅ Build → Test → Deploy will run again

Check deployment:

```bash
ls /tmp/github-cicd-deploy
cat /tmp/github-cicd-deploy/app.py
```

---

# ✅ FINAL ARCHITECTURE (EXAM ANSWER)

```
GitHub Repo
(app.py, test.py, Jenkinsfile)
        ↓
Poll SCM (every 1 minute)
        ↓
Jenkins Pipeline
        ↓
Build → Test → Deploy
        ↓
/tmp/github-cicd-deploy
```

---

# ✅ FINAL EXAM EXPLANATION (WRITE THIS)

> “I created a public GitHub repository containing app.py and test.py. I added a Jenkinsfile to define the CI/CD pipeline. I configured a Jenkins Pipeline job using ‘Pipeline script from SCM’ and enabled Poll SCM so Jenkins automatically checks for new commits. Whenever I push changes to GitHub, Jenkins automatically pulls the code and executes the Build, Test, and Deploy stages.”

---

# ✅ WHAT YOU HAVE NOW COMPLETELY DONE

✅ Public GitHub repo
✅ Only 2 files: app.py and test.py
✅ Jenkinsfile in repo
✅ Jenkins Pipeline from SCM
✅ Poll SCM auto-trigger
✅ Build stage
✅ Test stage
✅ Deploy stage
✅ Fully automatic CI/CD

---

If you want, I can also give you:

✅ **Same GitHub pipeline running on Jenkins Slave**
✅ **Webhook-based instant builds (without polling)**
✅ **Final record-format write-up for submission**

Tell me what you want next ✅


on slave in jenkinsFile

pipeline {
    agent {
        label 'slave1'
    }

