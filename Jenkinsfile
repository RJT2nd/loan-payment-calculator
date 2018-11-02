pipeline {
    agent {
        docker {
            image 'node:8'
        }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }
        stage('Build') {
            steps {
                sh 'ls'
                sh 'ls /var/lib/jenkins/workspace/loan-payment-app-pipeline/'
                sh 'npm install'
            }
        }
        stage('Deploy') {
            steps {
                sh 'npm run serve'
            }
        }
    }
}
