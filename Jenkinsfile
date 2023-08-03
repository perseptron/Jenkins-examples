pipeline {
    agent any
    environment {
        branch="${env.BRANCH_NAME}"
    }
    tools {
        nodejs 'node'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                echo "Current branch: ${branch}"
                git url: 'https://github.com/perseptron/cicd-pipeline', branch: 'main'
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
        stage('Docker build') {
            steps {
                sh 'docker build -t nodemain:v1.0 .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker rm -f nodemain:'
                sh 'docker run -d --name nodemain -p 3000:3000 nodemain:v1.0'
            }
        }
    }
}
