#### 🚀 AWS Service Node.js Project
   Containerized Node.js application with a complete CI/CD pipeline using Jenkins, Docker, and AWS EC2
#### ⭐ Project Highlights
This project demonstrates practical DevOps capability across:
- CI/CD automation using Jenkins scripted pipelines
- Building and managing Docker images
- Deploying services to AWS EC2 using Docker Compose
- Automated versioning and release workflows
- Secure remote deployments using SSH and Jenkins credentials
- It clearly shows hands-on experience with the core tools used in modern DevOps and Cloud roles.

#### 🧭 High-Level Architecture
 GitHub  →  Jenkins CI/CD  →  Docker Hub  →  AWS EC2  →  Docker Compose  →  Live App (port 3000)

#### 📌 Project Summary
This repository contains a Node.js application deployed through a fully automated CI/CD workflow.
Every push to the master branch triggers the pipeline to:
- Perform an automatic version bump
- Install dependencies and execute tests
- Build a Docker image
- Push the image to Docker Hub
- Deploy the updated version to AWS EC2 using secure SSH automation
- Commit the new version record back into GitHub
The entire process runs end-to-end without manual intervention.

#### 🧱 Key Components
| Component               | Purpose                                                             |
| ----------------------- | ------------------------------------------------------------------- |
| **Jenkinsfile**         | Automates testing, building, pushing, and AWS deployment            |
| **Dockerfile**          | Defines the container image for the Node.js application             |
| **docker-compose.yaml** | Runs the application on EC2 using the latest pushed image           |
| **server-cmd.sh**       | Deployment script executed on EC2 to update and restart the service |
| **/app**                | Node.js application source code                                     |

#### 🤖 CI/CD Pipeline Overview
1️⃣ Version Automation
-  Automatically bumps the minor version using npm and generates an image tag:
    <app-version>-<jenkins-build-number>
2️⃣ Install Dependencies & Run Tests
   Validates application behavior before building and deploying.
3️⃣ Build & Push Docker Image
   Packages the application as a container image and uploads it to Docker Hub.
4️⃣ Deploy to AWS EC2
    Jenkins securely transfers deployment files and triggers Docker Compose remotely to apply the new version.
5️⃣ Commit Version Bump to GitHub
   Pushes the updated package.json version back into the repository for visibility and traceability.

#### 📁 Repository Structure
aws-service-nodejs-project/
│
├── app/
├── Dockerfile
├── docker-compose.yaml
├── server-cmd.sh
└── Jenkinsfile

