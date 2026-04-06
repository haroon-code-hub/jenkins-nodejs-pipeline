pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Jenkins pipeline is running'
            }
        }
        stage('Checkout'){
            steps{
                checkout scm
            }
        }
         stage('Debug Environment') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'echo $PATH'
                sh 'which node || true'
                sh 'which npm || true'
                sh 'node --version || true'
                sh 'npm --version || true'
            }
        }
    }
}