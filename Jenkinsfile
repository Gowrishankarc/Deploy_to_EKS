pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Jenkins credentials ID for Docker Hub
        DOCKERHUB_REPO = '8197495215/i18next-app'        // Your Docker Hub repository
        IMAGE_TAG = 'latest'                             // Image tag
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo 'Cloning the repository...'
                }
                git branch: 'main', url: 'https://github.com/Gowrishankarc/Deploy_to_EKS.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh 'docker build -t $DOCKERHUB_REPO:$IMAGE_TAG .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo 'Logging into Docker Hub...'
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    echo 'Pushing image to Docker Hub...'
                    sh 'docker push $DOCKERHUB_REPO:$IMAGE_TAG'
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed to Docker Hub successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
