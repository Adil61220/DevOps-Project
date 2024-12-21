pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Adil61220/DevOps-Project.git'
        APP_NAME = 'todo-app'
        WORKSPACE_ARCHIVE = 'project.tar.gz'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    try {
                        echo "Checking out code from ${REPO_URL}"
                        git branch: 'main', url: "${REPO_URL}"
                        sh 'ls -la'
                    } catch (Exception e) {
                        error "Failed to checkout code: ${e.message}"
                    }
                }
            }
        }

        stage('Prepare Deployment') {
            steps {
                script {
                    try {
                        echo "Creating project archive..."
                        // Exclude unnecessary files from the archive
                        sh '''
                            tar -czf ${WORKSPACE_ARCHIVE} \
                                --exclude='.git' \
                                --exclude='node_modules' \
                                --exclude='dist' \
                                --exclude='*.tar.gz' \
                                .
                        '''
                        echo "Project archive created successfully"
                    } catch (Exception e) {
                        error "Failed to prepare deployment: ${e.message}"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    try {
                        echo "Starting deployment using Ansible..."
                        sh '''
                            ansible --version
                            ansible-playbook -i inventory.ini deploy.yml -v
                        '''
                        echo "Deployment completed successfully"
                    } catch (Exception e) {
                        error "Failed to deploy: ${e.message}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "=== Deployment Successful ==="
            echo "The application should now be accessible"
        }
        failure {
            echo "=== Deployment Failed ==="
            echo "Please check the logs for details"
        }
        always {
            echo "Cleaning up workspace..."
            sh 'rm -f ${WORKSPACE_ARCHIVE}'
            cleanWs()
        }
    }
}
