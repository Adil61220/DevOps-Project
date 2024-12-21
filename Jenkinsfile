pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Adil61220/DevOps-Project.git'
        DOCKER_IMAGE_APP = 'todo-app:latest'
        DOCKER_IMAGE_NGINX = 'todo-nginx:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
                sh 'ls -la'  // Debug: List files after checkout
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker Images') {
            steps {
                // Build app image
                sh 'docker build -t ${DOCKER_IMAGE_APP} -f Dockerfile .'
                // Build nginx image
                sh 'docker build -t ${DOCKER_IMAGE_NGINX} -f Dockerfile.nginx .'
            }
        }

        stage('Save Docker Images') {
            steps {
                // Save both images as tar files
                sh 'docker save -o todo-app.tar ${DOCKER_IMAGE_APP}'
                sh 'docker save -o todo-nginx.tar ${DOCKER_IMAGE_NGINX}'
                archiveArtifacts artifacts: '*.tar', allowEmptyArchive: false
            }
        }

        stage('Deploy Using Ansible') {
            steps {
                // Run ansible playbook
                sh 'ansible-playbook -i inventory.ini deploy.yml'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }
        failure {
            echo 'Deployment Failed'
        }
        always {
            // Clean up tar files
            sh 'rm -f *.tar'
            // Clean workspace
            cleanWs()
        }
    }
}
