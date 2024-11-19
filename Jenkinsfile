pipeline {
    agent any

    environment {
        
        DOCKER_CREDENTIALS = 'docker-hub-credentials'
        DOCKER_IMAGE = 'your-username/your-repository'  // dockerhub username and repo name 
        BRANCH_NAME = "main"             
        COMMIT_HASH = "${sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()}" 
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                   
                    git branch: "main", url: 'https://github.com/your-username/your-repository.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    
                    sh """
                    docker build -t ${DOCKER_IMAGE}:${COMMIT_HASH} -t ${DOCKER_IMAGE}:${BRANCH_NAME} .
                    """
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                   
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh """
                        docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                        docker push ${DOCKER_IMAGE}:${COMMIT_HASH}
                        docker push ${DOCKER_IMAGE}:${BRANCH_NAME}
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
