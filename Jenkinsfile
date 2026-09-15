pipeline {
    agent any

    tools {
        nodejs 'node'
        dockerTool 'docker'
    }

    stages {
        stage("Build") {
            steps {
                sh 'npm install'
            }
        }

        stage("Test") {
            steps {
                sh 'npm test'
            }
        }

        // ================= MAIN BRANCH =================
        stage("Deploy Main") {
            when {
                branch "main"
            }
            steps {
                sh "docker build -t nodemain:v1.0 ."


                sh "docker rm -f nodemain-app || true"

                sh "docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0"
            }
        }

        // ================= DEV BRANCH =================
        stage("Deploy Dev") {
            when {
                branch "dev"
            }
            steps {
                sh "docker build -t nodedev:v1.0 ."

                sh "docker rm -f nodedev-app || true"

                sh "docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0."
            }
        }
    }
}