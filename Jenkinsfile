pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '463470947695'
        REGION = 'us-east-1'
        REPO_NAME = 'my-devops-repo'
        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/Yogesh16091998/static-website-example.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-website .'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
aws ecr get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin 463470947695.dkr.ecr.us-east-1.amazonaws.com
'''
            }
        }

        stage('Tag Image') {
            steps {
                sh '''
                docker tag my-website:latest $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                docker push $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}
