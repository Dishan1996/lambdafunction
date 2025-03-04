pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1' // Replace with your AWS region
        LAMBDA_FUNCTION_NAME = 'eventretriver' // Replace with your Lambda function name
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r function.zip .'
            }
        }

        stage('Deploy to Lambda') {
            steps {
                withAWS(credentials: '5c39b478-2b74-4f34-82f6-f5c47a75595c', region: env.AWS_DEFAULT_REGION) {
                    sh "aws lambda update-function-code --function-name ${env.LAMBDA_FUNCTION_NAME} --zip-file fileb://function.zip"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
