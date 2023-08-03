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
                   if (env.BRANCH_NAME == 'main' ) { 
                       imgname='nodemain'
                   } else if (env.BRANCH_NAME == 'dev' ) {
                       sh 'mv src/tmp.svg src/logo.svg'
                       imgname='nodedev'
                   }
                }
                sh 'docker build -t ${imgname}:v1.0 .'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main' ) {
                        imgname='nodemain'
                        port=3000
                    } else if (env.BRANCH_NAME == 'dev' ) {
                        imgname='nodedev'
                        port=3001
                    }
                    sh 'docker rm -f ${imgname}' 
                    sh 'docker run -d --name ${imgname} -p ${port}:3000 ${imgname}:v1.0'
                }
            }
        }
    }
}
