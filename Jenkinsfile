pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = '651770526874.dkr.ecr.us-east-1.amazonaws.com/eks-ecr'
        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/rathnam009/restro.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t eks-ecr .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag eks-ecr:latest 651770526874.dkr.ecr.us-east-1.amazonaws.com/eks-ecr:latest'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 651770526874.dkr.ecr.us-east-1.amazonaws.com '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
