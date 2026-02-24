🚀 Project Overview

This project demonstrates containerization, CI/CD automation, and cloud deployment of a full-stack MEAN (MongoDB, Express, Angular, Node.js) application.

The application is deployed on AWS EC2 using:

Docker

Docker Compose

Jenkins (CI/CD)

Nginx reverse proxy

☁ Step 1: Create EC2 Instance

Launch EC2 instance

Select Ubuntu 22.04 LTS

Instance type: Any

Storage: 20 GB

Configure Security Group:

All traffic allowed (for testing purpose)

🛠 Step 2: Install Required Software
🔹 Update System
sudo apt update
🔹 Install Java (Required for Jenkins)
sudo apt install openjdk-17-jdk -y

Verify:

java -version
🔹 Install Git
sudo apt install git -y

Verify:

git --version
🔹 Install Docker
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker

Add user to Docker group:

sudo usermod -aG docker $USER

Logout and login again.

🔹 Install Docker Compose
sudo apt install docker-compose -y

Verify:

docker-compose --version
🔹 Install Jenkins
sudo apt install jenkins -y
Change Jenkins Port to 9090
sudo nano /etc/default/jenkins

Change:

HTTP_PORT=9090

Restart Jenkins:

sudo systemctl restart jenkins
Access Jenkins
http://<EC2-Public-IP>:9090

Unlock Jenkins:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Install suggested plugins and complete setup.

📂 Step 3: Clone Repository
git clone https://github.com/<your-username>/discoverdollar-mean-devops.git
cd discoverdollar-mean-devops
🔁 Step 4: Setup Jenkins Pipeline

Open Jenkins Dashboard

Click New Item

Select Pipeline

Enter pipeline name (e.g., mean-app-pipeline)

Click OK

🔹 Configure Pipeline
Under General:

Select GitHub hook trigger for GITScm polling

Under Pipeline:

Definition → Pipeline script

Copy and paste your Jenkins pipeline script

Click Apply

Click Save

🔔 Configure GitHub Webhook (Auto Trigger)

To trigger pipeline automatically on code changes:

Go to your GitHub repository

Click Settings → Webhooks → Add Webhook

Payload URL:

http://<EC2-Public-IP>:9090/github-webhook/

Content type:

application/json

Select:

Just the push event

Click Add Webhook.

🚀 Step 5: Run Pipeline

Click Build Now

Pipeline performs:

Clone repository

Build backend Docker image

Build frontend Docker image

Push images to Docker Hub

Deploy application using Docker Compose

You can monitor logs in:

Jenkins → Job → Console Output
🌐 Step 6: Access Application

Open browser:

http://<EC2-Public-IP>

Application runs on port 80 using Nginx reverse proxy.
