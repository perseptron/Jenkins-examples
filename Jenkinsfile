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
                       imgname='nodemain:v1.0'
                   } else if (env.BRANCH_NAME == 'dev' ) {
                       sh 'mv src/tmp.svg src/logo.svg'
                       imgname='nodedev:v1.0'
                   }
                }
                echo "imgname =  ${imgname}"
                echo "env.BRANCH_NAME = ${env.BRANCH_NAME}" 
                sh 'docker build -t ${imgname} .'
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
