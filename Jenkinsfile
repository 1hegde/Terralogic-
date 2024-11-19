pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub-credentials') // Jenkins credential id
        IMAGE_NAME = 'my-app-image'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Cloning the repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/1hegde/Terralogic-.git']])
                echo 'Code checkout done'
            }
        }
    
         stage('Build Image') {
            steps {
                // Build the Docker image
                sh 'docker build -t my-app-image:latest .'
                echo 'Docker image built'
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo ${DOCKER_PASSWORD} | docker login --username ${DOCKER_USERNAME} --password-stdin"
                    }

                    // Push the Docker image to Docker Hub
                    sh "docker push ${DOCKER_CREDENTIALS_USR}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
