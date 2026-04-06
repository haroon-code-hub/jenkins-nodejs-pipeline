pipeline {
    agent any

    tools {
        nodejs 'node20'
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Jenkins pipeline is running'
            }
        }

        stage('Check Node and npm') {
            steps {
                sh 'node --version'
                sh 'npm --version'
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
    }
}