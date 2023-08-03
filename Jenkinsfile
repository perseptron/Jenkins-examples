pipeline {
    agent any
    tools {
        nodejs 'node'
    }
    stages {
        stage('Checkout SCM') {
            steps {
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
                script {
                    if (env.BRANCH_NAME == 'main') {
                        echo 'main'
                    }
                }
                sh 'docker build -t nodemain:v1.0 .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker rm -f nodemain'
                sh 'docker run -d --name nodemain -p 3000:3000 nodemain:v1.0'
            }
        }
    }
}
