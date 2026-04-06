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
    }
}