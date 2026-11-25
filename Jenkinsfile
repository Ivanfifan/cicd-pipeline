pipeline {
    agent any

    options {
        skipDefaultCheckout()
        disableConcurrentBuilds()
    }

    tools {
        nodejs 'node'
    }

    stages {
        stage('Get code') {
            steps {
                script {
                    def gitInfo = checkout scm

                    if (gitInfo.GIT_BRANCH == 'main') {
                        env.APP_NAME = 'nodemain'
                        env.APP_PORT = '3000'
                    } else if (gitInfo.GIT_BRANCH == 'dev') {
                        env.APP_NAME = 'nodedev'
                        env.APP_PORT = '3001'
                    } else {
                        error("Unsupported branch: ${gitInfo.GIT_BRANCH}")
                    }
                }
            }
        }

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage("Build") {
            steps {
                sh 'docker build -t "${APP_NAME}:v1.0" .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    if docker container inspect "$APP_NAME" > /dev/null 2>&1; then
                        docker stop "$APP_NAME"
                        docker rm "$APP_NAME"
                    fi

                    docker run -d --name "$APP_NAME" \
                        -p "${APP_PORT}:3000" "${APP_NAME}:v1.0"
                '''
            }
        }
    }
}