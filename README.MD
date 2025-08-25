🚀 Spring Boot CI/CD with GitHub Actions

This project demonstrates a complete CI/CD pipeline for a Spring Boot application using:

GitHub Actions

Self-Hosted Runner on AWS

SonarQube (Self-hosted)

Trivy (Security scanner)

Gitleaks (Secrets detection)

JUnit (Unit testing)

Docker (Build & Push)

🏗️ CI/CD Workflow Overview
Stage	Tools Used	Description
Source Control	GitHub	Source code management
Runner	Self-hosted EC2 instance (Ubuntu)	Executes all GitHub workflows
Code Quality	SonarQube (on separate EC2 instance)	Static code analysis
Security Scanning	Trivy, Gitleaks	Detect vulnerabilities and secrets
Build	Maven	Compiles the code
Testing	JUnit	Runs unit tests
Containerization	Docker	Builds and pushes Docker images
☁️ Infrastructure Setup
🔸 1. Self-Hosted Runner (EC2 Instance)

Ubuntu EC2 instance (t2.medium or larger)

Installed: Java 17, Docker, unzip, wget, aws-cli, git

GitHub Actions runner configured by following GitHub's self-hosted runner guide

🔸 2. SonarQube Server (EC2 Instance)

Ubuntu EC2 instance (t2.large or larger)

Installed: Java, SonarQube, PostgreSQL (optional)

SonarQube accessible at: http://<your-sonarqube-ec2-ip>:9000

🔐 GitHub Repository Secrets & Variables
Key	Type	Description
SONAR_TOKEN	Secret	Token generated from SonarQube
SONAR_HOST_URL	Variable	URL of your SonarQube server (e.g., http://<IP>:9000)
DOCKER_USERNAME	Secret	Docker Hub or ECR username
DOCKER_PASSWORD	Secret	Docker Hub or ECR password
REGISTRY_URL	Variable	Docker registry URL (e.g., docker.io/<your-username>)
🔄 GitHub Actions Workflow Summary

The following workflows are triggered on every push to the main branch.

1. compile

Sets up Java 17

Runs mvn compile

2. security-check

Installs and runs Trivy and Gitleaks

Outputs fs-report.json and gitleaks-report.json

3. test

Runs unit tests using mvn test

4. build_project_and_sonar_scan

Builds JAR file with mvn package

Uploads the artifact

Runs SonarQube Scan

Checks SonarQube Quality Gate

5. build_docker_image_and_push (You should define this if not done yet)

Builds a Docker image

Tags it and pushes it to Docker Hub or ECR

📁 Directory Structure
.
├── src/
├── pom.xml
├── sonar-project.properties
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── Dockerfile
└── README.md