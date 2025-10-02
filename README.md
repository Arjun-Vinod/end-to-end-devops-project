# End to End DevOps Project

### Project Overview
This project demonstrates a complete end-to-end DevOps implementation of a microservices-based voting application. The pipeline involves continuous integration, automated testing, and deployment across QA and production environments using industry-standard DevOps tools and practices.

### Application Architecture
The voting application consists of five interconnected services:

- Voting App (Python): Frontend interface for users to cast votes.
- Redis: In-memory database for temporary vote storage.
- Worker App (.NET): Background service to process and validate votes.
- PostgreSQL: Persistent database for storing processed votes.
- Result App (Node.js): Real-time results visualization.

### Technology Stack

- **CI/CD:** Jenkins
- **Configuration Management:** Ansible
- **Containerization:** Docker
- **Container Registry:** Docker Hub
- **Version Control:** Git
- **Testing:** Selenium
- **Orchestration:** Kubernetes

### Setup Instructions

1. **Jenkins Server Setup**

- Install Java.
```bash
sudo apt update
sudp apt install -y openjdk-17-jdk
```
- Install Jenkins.
- Install Docker on the Jenkins server to enable image building.
```bash
 curl -fsSL https://get.docker.com -o install-docker.sh
 sudo sh install-docker.sh
```
- Establish ssh connection to Ansible Controller.

2. **Ansible Setup**

- Provision one Ansible Controller Node and two Managed Nodes (QA servers).
- Install Ansible on the Controller.
  ```bash
   $ sudo apt update
   $ sudo apt install software-properties-common
   $ sudo add-apt-repository --yes --update     ppa:ansible/ansible
   $ sudo apt install ansible
   ```
- Configure SSH from Controller to Managed Nodes.
- Add Managed Node IPs to `/etc/ansible/hosts` on the Controller.
- Create Playbooks:
   - `installDocker.yml`: Installs Docker on QA servers.
   - `deploy-containers-qa.yml`: Pulls images from Docker Registry (including Redis and PostgreSQL) and creates/runs containers on QA servers.

3. **Kubernetes Cluster Setup (via KOPS)**

- Provision a KOPS server with 1 Master and 2 Worker nodes.
- Install KOPS and kubectl on the KOPS server.
- Create Kubernetes manifest files:

- Deployment YAMLs for each service (using images from Docker Registry).
- Service YAMLs to expose the apps (e.g., LoadBalancer for Voting and Result Apps).

4. **Docker Registry Configuration**

- Create an account on Docker Hub.
- Ensure images are tagged as `username/image-name`.

### Jenkins Pipeline Stages

The Jenkins pipeline is multi-stage and automated. Below is a breakdown:

1. **Download**
-  Clone the git repo.

2. **Build Docker Images:**

- Pull Dockerfiles from Git repository.
- Build images for Voting App (Python), Worker App (.NET), and Result App (Node.js).


3. **Push to Docker Registry:**

- Login to Docker Registry using credentials.
- Tag and push built images (format: username/image-name).


4. **Deploy to QA (via Ansible):**

- SSH from Jenkins to Ansible Controller.
- Execute Ansible playbooks to:
   - Install Docker on QA servers.
   - Pull images (including Redis and PostgreSQL from public registry).
   - Create and run containers on QA servers.

5. **Automated Testing:**

- Download Selenium test scripts from Git.
- Execute Selenium scripts to test the QA-deployed containers.

6. **Deploy to Production (Kubernetes):**

- SSH from Jenkins to KOPS server.
- Run kubectl apply on deployment and service YAML manifests to deploy to the Kubernetes cluster.

## Key Learning
- Hands-on experience in building end-to-end DevOps pipelines.
- Improved skills in automation, orchestration
