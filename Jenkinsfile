pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        EC2_HOST = credentials('EC2_HOST')
        EC2_USERNAME = credentials('EC2_USERNAME')
        PRIVATE_KEY = credentials('PRIVATE_KEY')
    }
     
    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/YOUR-USERNAME/nodejs-cicd.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t nodejs-app .'
            }
        }
        stage('Deploy') {
            steps {
                sshagent (credentials: ['PRIVATE_KEY']) {
                    sh 'scp -o StrictHostKeyChecking=no docker-compose.yml $EC2_USERNAME@$EC2_HOST:/home/ubuntu/'
                    sh 'ssh -o StrictHostKeyChecking=no $EC2_USERNAME@$EC2_HOST "docker-compose up -d"'
                }
            }
        }
    }
}
pipeline {
    agent any
    environment {
        EC2_HOST = '3.85.214.123'  // Replace with your EC2 instance IP
    }
    stages {
        stage('Deploy') {
            steps {
                sh "ssh ubuntu@${EC2_HOST} 'docker pull myapp && docker run -d -p 80:3000 myapp'"
            }
        }
    }
}
