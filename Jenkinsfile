pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Jenkins credentials ID for Docker Hub
        DOCKERHUB_REPO = '8197495215/i18next-app'        // Your Docker Hub repository
        IMAGE_TAG = 'latest'                             // Image tag
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID') // AWS credentials ID
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY') // AWS credentials ID
        AWS_DEFAULT_REGION = "us-east-1"                // AWS region
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

        stage("Deploy to EKS") {
            steps {
                script {
                    echo 'Deploying to EKS...'
                    sh "aws eks update-kubeconfig --name abhi-eks-O5s37h8p"
                    sh "kubectl apply -f i18next-app_deployment.yaml --validate=false"

                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed to Docker Hub successfully!'
            echo 'Deployed to EKS cluster successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
