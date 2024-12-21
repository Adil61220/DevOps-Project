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
                script {
                    try {
                        git branch: 'main', url: "${REPO_URL}"
                        sh 'ls -la'
                    } catch (Exception e) {
                        error "Failed to checkout code: ${e.message}"
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    try {
                        // Build app image with build stage
                        sh '''
                            # Build the app image
                            docker build -t ${DOCKER_IMAGE_APP} -f Dockerfile .
                            
                            # Create a temporary container to copy the build files
                            docker create --name temp_container ${DOCKER_IMAGE_APP}
                            
                            # Copy the build files from the container
                            docker cp temp_container:/app/dist ./dist
                            
                            # Remove the temporary container
                            docker rm temp_container
                            
                            # Build nginx image with the build files
                            docker build -t ${DOCKER_IMAGE_NGINX} -f Dockerfile.nginx .
                        '''
                    } catch (Exception e) {
                        error "Failed to build Docker images: ${e.message}"
                    }
                }
            }
        }

        stage('Save Docker Images') {
            steps {
                script {
                    try {
                        // Save both images as tar files
                        sh 'docker save -o todo-app.tar ${DOCKER_IMAGE_APP}'
                        sh 'docker save -o todo-nginx.tar ${DOCKER_IMAGE_NGINX}'
                        archiveArtifacts artifacts: '*.tar', allowEmptyArchive: false
                    } catch (Exception e) {
                        error "Failed to save Docker images: ${e.message}"
                    }
                }
            }
        }

        stage('Deploy Using Ansible') {
            steps {
                script {
                    try {
                        sh 'ansible --version'
                        sh 'ansible-playbook -i inventory.ini deploy.yml'
                    } catch (Exception e) {
                        error "Failed to deploy using Ansible: ${e.message}"
                    }
                }
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
            // Clean up tar files, build directory and workspace
            sh '''
                rm -f *.tar
                rm -rf dist
            '''
            cleanWs()
        }
    }
}
