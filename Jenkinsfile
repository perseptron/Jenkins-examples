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
        stage('Docker build') {
            steps {
                script {
                   if ( ${env.BRANCH_NAME} == 'main' ) { 
                       sh 'docker build -t nodemain:v1.0 .'
                   } else if ( ${env.BRANCH_NAME} == 'dev' ) {
                       sh 'mv src/tmp.svg src/logo.svg'
                       sh 'docker build -t nodedev:v1.0 .'
                   }
                }
            }
        }
    }
}
