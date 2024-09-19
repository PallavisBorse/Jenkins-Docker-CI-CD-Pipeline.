# Jenkins-Docker-CI-CD-Pipeline
A CI/CD pipeline using Jenkins, Docker, and GitHub webhook integration to automatically deploy a Dockerized application on AWS EC2 instances.

# Project Overview

This project demonstrates how to set up a CI/CD pipeline using Jenkins and Docker, with GitHub webhook integration to automate deployments on AWS EC2. The pipeline builds, tests, and deploys a Dockerized application each time new code is pushed to the GitHub repository.

# Features

- Jenkins CI/CD Pipeline: Automates the build, test, and deployment processes.
- Docker: Containerizes the application for consistent environments.
- GitHub Webhooks: Triggers the pipeline automatically on code changes.
- AWS EC2: Hosts the Dockerized application for deployment.

# Prerequisites

- Jenkins: Make sure Jenkins is installed and running.
- Docker: Install Docker on the Jenkins server.
- AWS EC2: Provision an EC2 instance for deployment.
- GitHub Repository: The project code should be hosted on GitHub.

# Installation

1. **Clone the Repository**:
    ```bash
    git clone https://github.com/your-username/Jenkins-Docker-CI-CD-Pipeline.git
    cd Jenkins-Docker-CI-CD-Pipeline
    ```

2. **Set Up Jenkins**:
    - Follow the [Jenkins installation guide](https://www.jenkins.io/doc/book/installing/) to install Jenkins on your server.
    - Install necessary plugins (e.g., Docker, GitHub Integration).

3. **Configure Docker**:
    - Install Docker by following the [Docker installation guide](https://docs.docker.com/get-docker/).

4. **Create and Configure EC2 Instance**:
    - Launch an EC2 instance and configure security groups to allow HTTP and HTTPS traffic.
    - Ensure that Docker is installed on the EC2 instance.

5. **Set Up GitHub Webhooks**:
    - Go to your GitHub repository settings.
    - Navigate to **Webhooks** and add a new webhook with the payload URL pointing to your Jenkins server.

# Usage

1. **Build and Test**:
    - Configure your Jenkins pipeline to build and test the Dockerized application.

2. **Deploy**:
    - Jenkins will automatically deploy the application to the AWS EC2 instance after a successful build.

3. **Verify**:
    - Access your application using the public IP of the EC2 instance.

# Configuration

-  Jenkinsfile : Define the CI/CD pipeline stages.
-  Dockerfile : Configure the Docker image for your application.
-  deploy.sh : Script for deploying the Docker container to the EC2 instance.

## Troubleshooting

-  Jenkins Issues : Ensure Jenkins has the correct permissions and plugins.
-  Docker Issues : Verify Docker is correctly installed and configured.
- **AWS Deployment**: Check the security group settings and instance health.
