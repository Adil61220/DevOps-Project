# DevOps Project - React Todo App Deployment

This project demonstrates a complete CI/CD pipeline for deploying a React-based Todo application using Jenkins, Docker, and Ansible. The application is based on the open-source project [TodoApp](https://github.com/maciekt07/TodoApp).

## Architecture Overview

### CI/CD Pipeline Flow

1. **Source Code (GitHub)**
   → Clone repository to Jenkins
   
2. **Jenkins Pipeline**
   → Triggers Ansible deployment
   
3. **Ansible Controller**
   → Configures worker node
   
4. **Worker Node**
   → Builds and deploys Docker container

### Container Build Process

1. **Source Code**
   → React application code
   
2. **Build Stage (Node.js Container)**
   - Install Node.js dependencies
   - Build React application
   - Generate static files
   
3. **Production Stage (Nginx Container)**
   - Copy built files
   - Configure Nginx
   - Serve application

### Container Architecture

1. **Docker Host (Worker Node)**
   - Runs Docker daemon
   - Manages containers and networks

2. **Docker Network (todo_network)**
   - Isolates application containers
   - Enables container communication

3. **Application Container (todo-app)**
   - Based on Nginx Alpine
   - Serves static files from /usr/share/nginx/html
   - Exposes port 1080 externally → 80 internally

4. **Data Flow**
   - Client Request → Port 1080
   - Nginx → Serves Static Files
   - React App → Handles Client-side Logic

## Features

### Original Application Features
- 🔗 Share Tasks by Link or QR Code
- 🎨 Color Themes with Light/Dark Mode
- 📥 Import/Export Tasks
- 📴 Progressive Web App (PWA)
- 📱 Responsive Design
- For more features, visit the [original project](https://github.com/maciekt07/TodoApp)

### DevOps Implementation Features
- 🔄 Automated CI/CD Pipeline
- 🐳 Docker Containerization
- 🚀 Zero-downtime Deployment
- 🔍 Health Checks and Verification
- 🛠️ Automated Dependencies Installation
- 📊 Deployment Status Monitoring

## Technology Stack

### Infrastructure
- **Source Control**: GitHub
- **CI/CD**: Jenkins
- **Configuration Management**: Ansible
- **Containerization**: Docker
- **Web Server**: Nginx (built into container)

### Application
- **Frontend**: React.js
- **Build Tool**: Vite
- **Language**: TypeScript
- **UI Framework**: Material-UI (MUI)

## Project Structure
```
.
├── Dockerfile              # Multi-stage Docker build configuration
├── Jenkinsfile            # Jenkins pipeline definition
├── deploy.yml             # Ansible deployment playbook
├── inventory.ini          # Ansible inventory configuration
├── nginx.conf             # Nginx server configuration
├── src/                   # Application source code
```

## Detailed Workflow

### 1. Source Control (GitHub)
- Code changes pushed to repository
- Webhook triggers Jenkins pipeline
- Branch: main

### 2. Jenkins Pipeline
```groovy
Stages:
1. Verify Tools
   - Check Ansible
   - Check Git

2. Deploy with Ansible
   - Run playbook
   - Monitor execution
```

### 3. Ansible Controller
```yaml
Tasks:
1. Setup Worker
   - Install dependencies
   - Configure Docker

2. Build Process
   - Clone repository
   - Build Docker image
   - Multi-stage build process

3. Deployment
   - Create network
   - Run container
   - Health checks
```

### 4. Docker Build
```dockerfile
# Stage 1: Build
FROM node:20-alpine
- Install dependencies
- Build application

# Stage 2: Production
FROM nginx:alpine
- Copy build files
- Configure Nginx
- Expose port 80
```

### 5. Container Deployment
```yaml
Container Configuration:
- Image: todo-app:latest
- Port: 1080:80
- Network: todo_network
- Environment: production
```

## Setup Instructions

### Prerequisites
1. Jenkins Server Requirements:
   ```bash
   # Install required tools on Jenkins server
   sudo apt-get update
   sudo apt-get install -y ansible git
   ```

2. Worker Node Requirements:
   - SSH access
   - Python 3 (will be installed by Ansible)
   - Sudo privileges

### Jenkins Pipeline Setup

1. Create New Pipeline:
   - Open Jenkins Dashboard
   - New Item → Pipeline
   - Configure Git SCM with repository URL

2. Configure Credentials:
   - Add SSH credentials for worker node
   - Add GitHub credentials if repository is private

3. Configure Pipeline Settings:
   - Script Path: `Jenkinsfile`
   - Scan Repository Triggers (optional)

### Worker Node Configuration

1. Update inventory.ini with your worker node details:
   ```ini
   [worker]
   worker_ip ansible_port=22 ansible_user=your_user ansible_password=your_password

   [worker:vars]
   ansible_python_interpreter=/usr/bin/python3
   ansible_ssh_common_args='-o StrictHostKeyChecking=no'
   ansible_become=yes
   ansible_become_method=sudo
   ansible_become_pass=your_sudo_password
   ```

## Usage

1. **Trigger Deployment**:
   - Manual: Click "Build Now" in Jenkins
   - Automatic: Configure webhook triggers

2. **Monitor Deployment**:
   - Jenkins Console Output
   - Ansible Deployment Logs
   - Container Logs on Worker

3. **Access Application**:
   ```
   http://worker_ip:1080
   ```

## Troubleshooting

1. **Jenkins Pipeline Failures**:
   - Check Jenkins console output
   - Verify Ansible is installed
   - Ensure proper permissions

2. **Ansible Deployment Issues**:
   - Check worker node connectivity
   - Verify sudo privileges
   - Check Docker service status

3. **Container Issues**:
   - Check Docker logs:
     ```bash
     docker logs todo-app
     ```
   - Verify port availability
   - Check container status:
     ```bash
     docker ps -a
     ```

4. **Build Issues**:
   - Check multi-stage build logs:
     ```bash
     docker build --progress=plain .
     ```
   - Verify Node.js build:
     ```bash
     docker exec todo-app ls -la /usr/share/nginx/html
     ```

## Contributing
1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request
