✅ I Did This Using the Following Steps
🛠️ 1. Project Setup
Created a GitHub repo: dockerized-app-jenkins-pipeline

Inside it, added a simple HTML site and a Dockerfile in my-app/:

index.html

Dockerfile

jenkinsfile

README.md

☁️ 2. Azure VM Setup
Launched a Linux-based VM (Ubuntu) on Microsoft Azure with 4GB RAM.

Installed required tools on the VM:

bash
Copy
Edit
sudo apt update && sudo apt install docker.io git openjdk-11-jdk -y
🔧 3. Installed Jenkins
Installed Jenkins and started the service:

bash
Copy
Edit
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
Opened Jenkins in browser via: http://<your-server-ip>:8080

Unlocked Jenkins using:

bash
Copy
Edit
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
🔑 4. Docker Permissions
Added Jenkins user to Docker group to allow Jenkins to run Docker commands:

bash
Copy
Edit
sudo usermod -aG docker jenkins
sudo systemctl restart docker
sudo reboot  # or log out and back in
⚙️ 5. Configured Jenkins Pipeline
Created a new Pipeline Project in Jenkins called static-site-docker-pipeline.

Set GitHub repo URL: https://github.com/korinelu/dockerized-app-jenkins-pipeline

Jenkinsfile path set as: my-app/Jenkinsfile

🔁 6. Pipeline Stages in Jenkinsfile
Checkout Code from GitHub

Build Docker Image using the Dockerfile

Push to DockerHub:

DockerHub username: korinelu

Added DockerHub credentials in Jenkins and used them via withCredentials

Deploy:

Stopped any running container named static-site

Deployed a new one on port 8080

🧼 7. Fixes and Errors Handled
Fixed Docker permission error by adding Jenkins to Docker group.

Resolved DockerHub push denied issue by adding proper credentials in Jenkins.

Solved port conflict error: “address already in use on 8080” by stopping previous container using:

bash
Copy
Edit
docker stop static-site || true
docker rm static-site || true
🐳 DockerHub Image
👉 korinelu/static-site-demo on DockerHub


bash
Copy
Edit
docker pull korinelu/static-site-demo
docker run -d -p 8081:80 korinelu/static-site-demo

https://github.com/korinelu/dockerized-app-jenkins-pipeline

https://hub.docker.com/r/korinelu/static-site-demo




