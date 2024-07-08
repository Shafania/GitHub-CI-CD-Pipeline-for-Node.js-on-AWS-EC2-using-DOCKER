# GitHub-CI-CD-Pipeline-for-Node.js-on-AWS-EC2-using-DOCKER
This project demonstrates a practical implementation of a Continuous Integration and Continuous Deployment (CI/CD) pipeline using Jenkins and GitHub on an AWS EC2 instance. It includes setting up Jenkins, integrating it with GitHub, and automating the deployment of a Node.js application using Docker. 

# Prerequisites
*AWS Account: Ensure you have an AWS account.
*GitHub Repository: Have your Node.js project in a GitHub repository.
*AWS EC2 Instance: Set up an EC2 instance with the necessary security groups.
*Jenkins Installed: Jenkins should be installed and running on your EC2 instance.
*Docker Installed: Docker should be installed and configured on your EC2 instance.
*Jenkins Plugins: Install necessary Jenkins plugins (e.g., GitHub Integration, Docker Pipeline).

# Step-by-Step Setup

## Step 1: Set up AWS EC2 Instance
Launch EC2 Instance:
Sign in to AWS Console.
Navigate to EC2 Dashboard.
Launch a new EC2 instance with an appropriate AMI and instance type.
Configure instance details, add storage, configure security groups to allow necessary ports (e.g., 22 for SSH, 8080 for Jenkins, 8000 for the Node.js application).
Launch the instance.

## Step 2: Set up Jenkins on AWS EC2 Instance
Connect to EC2 Instance via SSH:

Open Jenkins in Web Browser:
Navigate to http://<your-ec2-public-ip>:8080.
Retrieve initial admin password:
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

## Step 3: Install Docker
Install Docker:
sudo apt install docker.io
sudo usermod -a -G docker $USER
Logout and Log back in to apply Docker group changes.

## Step 4: Create Dockerfile for Node.js Application
Create Dockerfile in project directory:
Dockerfile

FROM node:12.2.0-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 8000
CMD ["node", "app.js"]

## Step 5: Build and Run Docker Container Locally
Build Docker Image:

docker build . -t node-app
Run Docker Container:

docker run -d --name node-todo-app -p 8000:8000 node-app

## Step 6: Integrate Jenkins with GitHub
Generate GitHub Personal Access Token:

Go to GitHub > Settings > Developer settings > Personal access tokens.
Generate a new token with necessary scopes (repo, admin).
Configure GitHub Plugin in Jenkins:

Go to Jenkins > Manage Jenkins > Manage Plugins > Available Plugins.
Install "GitHub Integration" plugin.
Go to Jenkins > Manage Jenkins > Configure System.
Scroll to "GitHub" section > Add GitHub Server > Add credentials using the token.

## Step 7: Configure GitHub Webhook
Set up Webhook in GitHub Repository:

Go to GitHub repository > Settings > Webhooks > Add webhook.
Payload URL: http://<your-jenkins-server>/github-webhook/.
Content type: application/json.
Select events: Push events.
Configure Jenkins to Receive GitHub Webhook:

Go to Jenkins > Manage Jenkins > Configure System.
Scroll to "GitHub" section > Advanced > Let Jenkins auto-manage hook URLs.

## Step 8: Test the Setup
Make a small change to your code and push it to GitHub.
Check Jenkins Dashboard:
Verify if the job is triggered automatically.
Check the console output for build and deployment steps.
