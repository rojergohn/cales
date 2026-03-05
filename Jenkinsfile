pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = "205994119856.dkr.ecr.us-east-1.amazonaws.com/cals"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'cicdbranch', url: 'https://github.com/rojergohn/cales.git''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cals:$BUILD_NUMBER .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag cals:$BUILD_NUMBER $ECR_REPO:$BUILD_NUMBER'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin 205994119856.dkr.ecr.us-east-1.amazonaws.com
                '''
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh 'docker push $ECR_REPO:$BUILD_NUMBER'
            }
        }

    }
}
