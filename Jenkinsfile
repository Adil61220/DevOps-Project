pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Adil61220/DevOps-Project.git'
    }

    stages {
        stage('Verify Tools') {
            steps {
                script {
                    try {
                        echo "Verifying required tools..."
                        sh '''
                            ansible --version
                            git --version
                        '''
                    } catch (Exception e) {
                        error "Required tools are missing: ${e.message}"
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
                            # Run playbook with verbose output
                            ansible-playbook -i inventory.ini deploy.yml -v
                        '''
                        echo "Deployment triggered successfully"
                    } catch (Exception e) {
                        error "Failed to deploy: ${e.message}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo """
            ===================================
            ✅ Deployment Successful
            ===================================
            The application should now be accessible at:
            http://worker:1080
            
            Please allow a few moments for the container to fully start.
            """
        }
        failure {
            echo """
            ===================================
            ❌ Deployment Failed
            ===================================
            Please check:
            1. Jenkins console output
            2. Ansible logs
            3. Worker node Docker logs
            """
        }
        always {
            cleanWs()
        }
    }
}
