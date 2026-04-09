# Jenkins_Issues
Good — now your repo issue is fixed ✅
This new error is **very common in Jenkins** 👇

---

## ❌ **Actual Problem**

```id="mvnerr1"
mvn: not found
```

👉 Jenkins is trying to run **Maven**, but Maven is **not installed** in your Jenkins environment.

---

## 🧠 **Why this happens**

You are running Jenkins (likely in Docker), and:

* Jenkins container has **Java ❌ Maven**
* Your pipeline runs:

```bash
mvn clean package
```

👉 But system doesn’t know what `mvn` is → ❌

---

## ✅ **3 Ways to Fix (choose one)**

---

### 🔥 **Option 1: Install Maven inside Jenkins container (quick fix)**

Run inside your Jenkins container:

```bash
apt update
apt install maven -y
```

Then restart build ✅

---

### 🚀 **Option 2: Configure Maven in Jenkins (best practice)**

1. Go to Jenkins Dashboard
2. Manage Jenkins → Global Tool Configuration
3. Find **Maven**
4. Click **Add Maven**
5. Name: `Maven-3`
6. Tick **Install automatically**

Then update pipeline:

```groovy
tools {
    maven 'Maven-3'
}
```

---

### 🐳 **Option 3: Use Maven Docker image (recommended in DevOps)**

Instead of installing manually:

```groovy
pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-17'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```

👉 This is **real-world best practice**

---

## ⚠️ **Also Notice This Warning**

```id="gitwarn"
Selected Git installation does not exist. Using Default
```

👉 Not critical, but better fix:

* Go to **Global Tool Configuration**
* Add Git → auto install

---

## 🧩 **Summary**

| Issue         | Fix                              |
| ------------- | -------------------------------- |
| mvn not found | Install Maven / use Docker agent |
| Git warning   | Configure Git tool               |

---

## ✅ **Final Answer (one line)**

**Build failed because Maven is not installed in Jenkins — install Maven or use a Maven Docker agent.**

---

If you want, I can give you a **perfect working Jenkinsfile for your repo (Java + Docker build + run)** 👍

You’re inside a **Jenkins Docker container**, and this is the key issue 👇

---

## ❌ **Why your commands failed**

You tried:

```bash
yum install mvn
```

👉 Error:

```bash
yum: command not found
```

### 💡 Reason:

* Jenkins official image is based on **Debian/Ubuntu**
* So:

  * ❌ `yum` (used in Amazon Linux / CentOS)
  * ✅ `apt` (used in Debian/Ubuntu)

---

## ✅ **Correct command (use this)**

Run inside container:

```bash
apt update
apt install maven -y
```

---

## ❗ But wait — important DevOps concept

Even if this works, it's **NOT recommended** ❌

👉 Because:

* Container changes are **temporary**
* If container restarts → Maven gone 😓

---

## 🔥 **Best Practice (very important)**

### ✔ Option 1: Use Maven container (clean & real-world)

Update your Jenkinsfile:

```groovy
pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-17'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```

👉 No need to install anything manually ✅

---

### ✔ Option 2: Create custom Jenkins image (advanced)

Create Dockerfile:

```dockerfile
FROM jenkins/jenkins:lts

USER root
RUN apt update && apt install -y maven
USER jenkins
```

Then build & run:

```bash
docker build -t jenkins-maven .
docker run -p 8080:8080 jenkins-maven
```

---

## ⚡ Quick Fix (if you want immediate result)

Just run:

```bash
apt update && apt install -y maven
```

Then:

```bash
mvn -v
```

---

## 🧠 Simple Understanding

| Tool | OS Type               |
| ---- | --------------------- |
| yum  | Amazon Linux / CentOS |
| apt  | Ubuntu / Debian       |

👉 Your container = Debian → use **apt**

---

## ✅ **Final Answer (one line)**

**`yum` failed because Jenkins container uses Debian — use `apt install maven` or switch to Maven Docker agent (best).**

---

If you want, I can **convert your full pipeline into production-level CI/CD (Java + Docker + deploy)** 👍
