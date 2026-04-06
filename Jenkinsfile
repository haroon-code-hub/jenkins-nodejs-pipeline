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
        stage('Increment version') {
            steps {
                sh 'npm version minor --no-git-tag-version'
            }
        }
        stage('Read version') {
            steps {
                script {
                    env.APP_VERSION = sh(
                        script: "node -p \"require('./package.json').version\"",
                        returnStdout: true
                    ).trim()
                }
                echo "Application version: ${env.APP_VERSION}"
            }
        }
        stage('Build Docker image') {
            steps {
                sh "docker build -t jenkins-nodejs-pipeline:${env.APP_VERSION} ."
            }
        }
    }
}