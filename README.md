# DevOps Project - Todo App Deployment

This project demonstrates a complete CI/CD pipeline for deploying a React-based Todo application using Jenkins, Docker, and Ansible. The application is based on the open-source project [TodoApp](https://github.com/maciekt07/TodoApp) by [maciekt07](https://github.com/maciekt07).

## Original Project Features

The Todo application includes several features from the original project:
- 🔗 Share Tasks by Link or QR Code
- 🎨 Color Themes with Light/Dark Mode
- 📥 Import/Export Tasks
- 📴 Progressive Web App (PWA)
- 📱 Responsive Design
- And many more features from the [original project](https://github.com/maciekt07/TodoApp)

## DevOps Workflow

### Architecture
```
GitHub (Source) → Jenkins Pipeline → Docker Build → Ansible Deploy → Worker Node
```

### Components
1. **Source Control**: GitHub
   - Repository: [DevOps-Project](https://github.com/Adil61220/DevOps-Project)
   - Based on: [TodoApp](https://github.com/maciekt07/TodoApp)

2. **CI/CD Pipeline**: Jenkins
   - Automated build and deployment process
   - Multi-stage pipeline with error handling
   - Artifact management

3. **Containerization**: Docker
   - Multi-container setup (App + Nginx)
   - Volume management for persistence
   - Custom networking between containers

4. **Configuration Management**: Ansible
   - Automated deployment to worker nodes
   - Infrastructure as Code
   - Zero-downtime deployment

### Workflow Steps
1. **Source Code Management**
   - Code is pulled from GitHub
   - Dependencies are installed
   - Application is built

2. **Docker Image Creation**
   - Two images are created:
     - Application image (Node.js)
     - Nginx image for serving static files
   - Images are saved as tar files

3. **Ansible Deployment**
   - Images are transferred to worker nodes
   - Docker network and volumes are created
   - Containers are deployed with proper configuration

4. **Production Setup**
   - Nginx serves the static files
   - Application runs in production mode
   - Automatic container restart on failure

## Project Structure
```
.
├── Dockerfile           # Application container configuration
├── Dockerfile.nginx     # Nginx container configuration
├── Jenkinsfile         # CI/CD pipeline definition
├── deploy.yml          # Ansible deployment playbook
├── inventory.ini       # Ansible inventory configuration
├── nginx.conf          # Nginx server configuration
└── docker-compose.yml  # Local development setup
```

## Requirements

### Jenkins Server
- Jenkins with Pipeline plugin
- Node.js and npm
- Docker
- Ansible

### Worker Node
- Docker
- Python 3
- SSH access

## Deployment

### 1. Jenkins Pipeline Setup
1. Create a new Pipeline job in Jenkins
2. Configure Git SCM with repository URL
3. Use the provided Jenkinsfile

### 2. Worker Node Setup
1. Ensure Docker is installed
2. Configure SSH access
3. Install Python 3 and Docker Python module

### 3. Running the Pipeline
1. Trigger the Jenkins pipeline
2. Pipeline will:
   - Build the application
   - Create Docker images
   - Deploy using Ansible

### 4. Accessing the Application
- The application will be available at `http://worker-ip:1080`
- Nginx serves the static files
- Application data persists through Docker volumes

## Configuration Files

### Jenkinsfile
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') { ... }
        stage('Install Dependencies') { ... }
        stage('Build App') { ... }
        stage('Build Docker Images') { ... }
        stage('Save Docker Images') { ... }
        stage('Deploy Using Ansible') { ... }
    }
}
```

### Ansible Inventory
```ini
[worker]
100.114.50.70 ansible_port=22 ansible_user=dev ansible_password=dev
```

## Credits
- Original Todo Application: [TodoApp](https://github.com/maciekt07/TodoApp) by [maciekt07](https://github.com/maciekt07)
- Live Demo of Original App: [react-cool-todo-app.netlify.app](https://react-cool-todo-app.netlify.app/)

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
