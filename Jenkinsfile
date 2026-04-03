pipeline {
    agent any

    environment {
        ACCOUNT_ID = "463470947695"
        REGION = "us-east-1"
        REPO = "my-devops-repo"
        IMAGE = "my-website"
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
        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 463470947695.dkr.ecr.us-east-1.amazonaws.com
        '''
    }
}

        stage('Tag Image') {
            steps {
                sh '''
                docker tag my-website:latest 463470947695.dkr.ecr.us-east-1.amazonaws.com/my-devops-repo:latest
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                docker push 463470947695.dkr.ecr.us-east-1.amazonaws.com/my-devops-repo:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop my-container || true
                docker rm my-container || true
                docker run -d -p 80:80 --name my-container my-website
                '''
            }
        }
    }
}
