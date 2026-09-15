pipeline {
    agent any

    tools {
      node 'node'
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
    }
}