**Project Setup: Jenkins-Docker-CI-CD-Pipeline **
This document provides a complete guide for setting up a Jenkins CI/CD pipeline with Docker and GitHub webhook integration to deploy a Dockerized application on AWS EC2 instances.

**Overview**
-Project Name: Jenkins-Docker-CI-CD-Pipeline
-Description: CI/CD pipeline using Jenkins, Docker, and GitHub webhook integration for 
 automatic deployment on AWS EC2 instances.

**Prerequisites**
**1. AWS EC2 Instance**
   - Name: jenkins-server
   - AMI: Ubuntu
   - Instance Type: t2.micro (free tier)
   - Key Pair Login: Create > docker.pem
   - Security Groups: Allow HTTP and HTTPS

**2. Software Requirements**
    - Jenkins: Installation Guide
    - Docker: Installation Guide

**Steps**
**1. Create AWS EC2 Instance**
       1. Go to AWS portal and create a new instance.
       2. Configure the instance as mentioned in the prerequisites.
       3. Download the .pem file for SSH access.
       
**2. Connect to EC2 Instance**
       1. Open the terminal and navigate to the folder where the .pem file is located.
       2. Run:
        **ssh -i "docker.pem" ubuntu@<EC2_PUBLIC_IP>**
        
**3. Set Up Jenkins and Docker**
     1. Generate SSH keys on the EC2 instance:
         **ssh-keygen**
     2. Install Jenkins and Docker:
        **sudo apt update**
        **sudo apt install openjdk-11-jdk**
        **sudo apt install wget**
        **wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -**
        **sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary > /etc/apt/sources.list.d/jenkins.list'**
        **sudo apt update**
        **sudo apt install jenkins**
        **sudo apt install docker.io**
     3. Check installations:
        **jenkins --version**
        **docker --version**
     4. Open Jenkins in a browser at http://<EC2_PUBLIC_IP>:8080 and follow the setup instructions:
        - Retrieve the initial admin password:
        **cat /var/lib/jenkins/secrets/initialAdminPassword**
        - Install suggested plugins and create the first admin user.
        
**4. Configure Jenkins**
     1. From Jenkins Dashboard, create a new pipeline job:
        - Click on “New Item” and enter the project name (e.g., todo-app).
        - Select “Pipeline” and click “OK”.
     2. Configure the pipeline:
        - Set up Source Code Management to use Git with your repository URL and credentials.
        - In the Pipeline section, define the pipeline script. Use the following Jenkinsfile content: 
        
pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'your-docker-image-name'
        AWS_EC2_IP = 'your-ec2-public-ip'
        AWS_SSH_KEY = 'path-to-your-ssh-key'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/your-repository.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshagent(['your-ssh-credentials-id']) {
                        sh """
                        ssh -i ${AWS_SSH_KEY} ubuntu@${AWS_EC2_IP} 'docker pull ${DOCKER_IMAGE} && docker run -d -p 80:80 ${DOCKER_IMAGE}'
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

    3. Set up GitHub Webhooks:
        - Go to GitHub repository settings.
        - Navigate to Webhooks and add a new webhook:
            Payload URL: http://<EC2_PUBLIC_IP>:8080/github-webhook/
            Content Type: application/json
            Which events would you like to trigger this webhook? Just the push event.
            Active: True
**5. Testing**
-Make changes to your code and push to GitHub.
-Jenkins should automatically build and deploy the application.

