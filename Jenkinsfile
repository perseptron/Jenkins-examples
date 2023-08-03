pipeline {
    agent any
    environment {
        branch="${env.BRANCH_NAME}"
        script {
            if ( if ${branch} == 'main' ) {
                img='nodemain'
                port=3000
                file='logo.svg'
            } else {
                img='nodedev'
                port=3001
                file='tmp.svg'
            }
        }
    }
    
    tools {
        nodejs 'node'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                echo "Current branch: ${branch}"
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
                sh 'mv src/${file} src/logo.svg
                sh 'docker build -t ${img}:v1.0 .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker rm -f ${img}:'
                sh 'docker run -d --name ${img} -p ${port}:3000 ${img}:v1.0'
            }
        }
    }
}
