pipeline {
    agent {
        label 'docker'
    }

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = '503459125865.dkr.ecr.us-east-1.amazonaws.com/kyc-app'
        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t kyc-app .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag kyc-app:latest $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin 503459125865.dkr.ecr.us-east-1.amazonaws.com
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push $ECR_REPO:$IMAGE_TAG'
            }
        }
    }
}