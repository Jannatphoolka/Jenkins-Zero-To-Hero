# Jenkins CI/CD Pipeline with Docker and Kubernetes

This project demonstrates a Continuous Integration/Continuous Deployment (CI/CD) pipeline for a Spring Boot application using Jenkins, Docker, and Kubernetes. It is hosted on AWS EC2 instances and integrates tools like SonarQube for code quality checks and ArgoCD for application deployment.

## Project Overview

- **Application:** Spring Boot application (source code from a GitHub repository).
- **Infrastructure:**
    - AWS EC2 instances:
        - `maven-ci-cd`: `t2.large`, Ubuntu, key: `mykey`.
        - `jenkins`: `t2.medium`, Ubuntu, key: `mykey`.
    - Docker Desktop with Kubernetes enabled for running ArgoCD.
- **Pipeline Components:**
    - Jenkins pipeline created using the Pipeline Script from SCM.
    - SonarQube integration for static code analysis.
    - ArgoCD for Kubernetes-based application deployment.

## Prerequisites

1. **AWS EC2 Setup:**
    - Two Ubuntu instances (AMI: Ubuntu):
        - **Instance 1 (maven-ci-cd):** `t2.large`.
        - **Instance 2 (jenkins):** `t2.medium`.
    - Edit inbound rules to allow all traffic (`0.0.0.0`) for necessary TCP ports.

2. **Local System Requirements:**
    - Installed software:
        - JDK, Java, and environment variables configured.
        - Docker Desktop (Kubernetes enabled).
        - Git Bash and PuTTY for command execution.
        - Windows PowerShell for setup commands.

3. **Installed Tools:**
    - Docker and Docker plugins.
    - Jenkins and Jenkins plugins (SonarQube, Kubernetes).
    - ArgoCD.

## Setup Steps

1. **Generate Key Pair:**
    - Create a key named `mykey` using PuTTY for SSH access to EC2 instances.

2. **Configure Instances:**
    - Install necessary dependencies on both EC2 instances:
        - JDK, Java, Docker, Jenkins.
    - Set up Jenkins on the `jenkins` instance and install required plugins (e.g., Docker, SonarQube).

3. **Build and Test Application:**
    - Clone the Spring Boot application repository.
    - Use `maven-ci-cd` EC2 instance for building the application using Maven (`pom.xml`).

4. **Set Up Jenkins Pipeline:**
    - Create a Jenkins pipeline using the "Pipeline Script from SCM" option.
    - Integrate with SonarQube for code quality analysis.

5. **Deploy with ArgoCD:**
    - Enable Kubernetes on Docker Desktop.
    - Use ArgoCD to deploy the application in a Kubernetes cluster.

6. **Alternate Setup (Minikube):**
    - Initially attempted using Minikube for Kubernetes but switched to Docker Desktop Kubernetes due to compatibility issues.

## Tools and Technologies Used

- **Jenkins**: CI/CD pipeline automation.
- **Docker**: Containerization platform for Jenkins and Kubernetes.
- **Kubernetes**: Managed using Docker Desktop for running ArgoCD.
- **SonarQube**: Static code quality analysis.
- **Git**: Source control and versioning.
- **PuTTY & Git Bash**: Command-line interfaces for interacting with AWS instances.

## Acknowledgments

- The Spring Boot application code is sourced from an open GitHub repository.
- Docker Desktop provided Kubernetes support for ArgoCD-based deployments.
- Special thanks to Jenkins and SonarQube communities for plugins and integrations.

## Argo CD Pipeline Deployment

![Argo CD Pipeline Deployment](C:\Users\HP\OneDrive\Documents\GitHub\Kubernetes-Native-CI-CD-Pipeline\image (2).png)

### Argo CD Pods

![Argo CD Pods](image (3).png)

### Docker Desktop Containers

![Docker Desktop Containers](image (4).png)

### Jenkins Dashboard

![Jenkins Dashboard](image (5).png)

### SonarQube: Port: 9000

SonarQube checks the code quality, and if the code is incorrect, the pipeline will fail and not proceed with the build.

## Next Steps

- Tests:
    - Unit tests.
    - End-to-end tests (external Selenium).

### PuTTY Setup

To set up SonarQube on the `maven-ci-cd` instance, run the following commands:

1. Install SonarQube:
    ```bash
    sudo snap install sonar
    ```

2. Find the SonarQube directory:
    ```bash
    sudo find / -type d -name "sonarqube"
    ```

3. Navigate to the SonarQube directory:
    ```bash
    cd /home/ubuntu/Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s/spring-boot-app/sonarqube
    ```

4. Install necessary dependencies and start SonarQube:
    ```bash
    sudo apt install unzip
    adduser sonarqube
    wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
    unzip *
    chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
    chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
    cd sonarqube-9.4.0.54424/bin/linux-x86-64/
    ./sonar.sh start
    ```

5. Edit `pom.xml` file:
    - Modify Jenkins URL from the manage path in settings of the Jenkins server.
    - Edit SonarQube URL in the `JenkinsFile` by using `nano JenkinsFile`.
![SonarQube](image (6).png)
### Inbound Rules Configuration for EC2 Instances

I set up inbound rules to allow traffic on certain HTTP, HTTPS, Custom TCP Ports on AWS Security Groups


### Jenkins Pipeline

The pipeline consists of the following stages:

1. **Checkout**
2. **Build and Test**
3. **Static Code Analysis**
4. **Build and Push**
5. **Update Deployment**

![Jenkins Pipeline](image (9).png)

## Conclusion

This CI/CD pipeline setup allows for efficient integration and deployment of a Spring Boot application using Jenkins, Docker, Kubernetes, and ArgoCD, with built-in code quality checks via SonarQube.
