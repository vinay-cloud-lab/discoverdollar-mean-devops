# 🚀 Project Overview

This project demonstrates containerization, CI/CD automation, and cloud deployment of a full-stack MEAN (MongoDB, Express, Angular, Node.js) application.

The application is deployed on AWS EC2 using:

- Docker
- Docker Compose
- Jenkins (CI/CD)
- Nginx Reverse Proxy

---

# ☁ Step 1: Create EC2 Instance

1. Launch EC2 instance
2. Select Ubuntu 22.04 LTS
3. Instance Type: Any
4. Storage: 20 GB
5. Configure Security Group:
   - Allow All Traffic (for testing purpose)

Connect to the EC2 instance using SSH.

---

# 🛠 Step 2: Install Required Software

## Update System

```bash
sudo apt update
```

## Install Java (Required for Jenkins)

```bash
sudo apt install openjdk-17-jdk -y
```

Verify:

```bash
java -version
```

## Install Git

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

## Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Add user to Docker group:

```bash
sudo usermod -aG docker $USER
```

Logout and login again.

## Install Docker Compose

```bash
sudo apt install docker-compose -y
```

Verify:

```bash
docker-compose --version
```

## Install Jenkins

```bash
sudo apt install jenkins -y
```

## Change Jenkins Port to 9090

```bash
sudo nano /etc/default/jenkins
```

Change:

```
HTTP_PORT=9090
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

Access Jenkins:

```
http://<EC2-Public-IP>:9090
```

Unlock Jenkins:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Install suggested plugins and complete setup.

---

# 📂 Step 3: Clone Repository

```bash
git clone https://github.com/<your-username>/discoverdollar-mean-devops.git
cd discoverdollar-mean-devops
```

---

# 🔁 Step 4: Setup Jenkins Pipeline

1. Open Jenkins Dashboard
2. Click New Item
3. Select Pipeline
4. Enter pipeline name (e.g., mean-app-pipeline)
5. Click OK

## Configure Pipeline

Under General:
- Select "GitHub hook trigger for GITScm polling"

Under Pipeline:
- Definition → Pipeline script
- Copy and paste your Jenkins pipeline script
- Click Apply
- Click Save

---

# 🔔 Configure GitHub Webhook (Auto Trigger)

1. Go to your GitHub repository
2. Click Settings → Webhooks → Add Webhook

Payload URL:

```
http://<EC2-Public-IP>:9090/github-webhook/
```

Content type:

```
application/json
```

Select:
- Just the push event

Click Add Webhook.

Now whenever code is pushed to GitHub, Jenkins will trigger automatically.

---

# 🚀 Step 5: Run Pipeline

Click Build Now in Jenkins.

Pipeline performs:

- Clone repository
- Build backend Docker image
- Build frontend Docker image
- Push images to Docker Hub
- Deploy application using Docker Compose

Monitor logs in:

Jenkins → Job → Console Output

---

# 🌐 Step 6: Access Application

Open browser:

```
http://<EC2-Public-IP>
```

The application runs on port 80 using Nginx reverse proxy.

---

# ✅ Final Result

- Application containerized using Docker
- CI/CD automated using Jenkins
- Images pushed to Docker Hub
- Deployment automated using Docker Compose
- Accessible via EC2 public IP on port 80
