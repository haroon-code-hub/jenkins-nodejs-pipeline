pipeline {
    agent any
    environment {
    DOCKER_IMAGE = 'saeedha/jenkins-nodejs-pipeline'
    }
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
                    sh "docker build -t ${DOCKER_IMAGE}:${APP_VERSION} ."

            }
        }
        stage('Push Docker image') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: 'docker-hub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {
                sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push ${DOCKER_IMAGE}:${APP_VERSION}
                '''
            }
        }
    }
    }
}