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
        stage('Check Node and npm') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
    }
}