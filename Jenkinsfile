pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'saeedha/jenkins-nodejs-pipeline'
    }

    stages {
        stage('Prepare') {
            steps {
                checkout scm
                echo 'Jenkins pipeline is running'
                sh 'node --version'
                sh 'npm --version'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Version') {
            steps {
                sh 'npm version minor --no-git-tag-version'
                script {
                    env.APP_VERSION = sh(
                        script: "node -p \"require('./package.json').version\"",
                        returnStdout: true
                    ).trim()
                }
                echo "Application version: ${env.APP_VERSION}"
            }
        }

        stage('Build and Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        docker build -t ${DOCKER_IMAGE}:${APP_VERSION} .
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:${APP_VERSION}
                    '''
                }
            }
        }

        stage('Commit Version') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'git-credentials',
                    usernameVariable: 'GIT_USER',
                    passwordVariable: 'GIT_PASS'
                )]) {
                    sh '''
                        git config user.email "jenkins@local"
                        git config user.name "Jenkins"
                        git add package.json package-lock.json
                        git commit -m "CI: bump version to ${APP_VERSION}" || echo "No changes to commit"
                        git push https://${GIT_USER}:${GIT_PASS}@github.com/haroon-code-hub/jenkins-nodejs-pipeline.git HEAD:main
                    '''
                }
            }
        }
    }
}