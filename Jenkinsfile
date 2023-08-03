pipeline {
    agent any
    tools {
        nodejs 'node'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/perseptron/cicd-pipeline'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
}
