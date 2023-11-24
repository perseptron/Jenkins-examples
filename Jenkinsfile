// CICD pipeline
pipeline {
    environment{
        registry="perseptron"
    }
    agent any
    tools {
        nodejs 'node'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/perseptron/Jenkins-examples', branch: 'main'
            }
        }

        stage ("lint dockerfile") {
            agent {
                docker {
                    image 'hadolint/hadolint:latest-debian'
                    reuseNode true
                }
            }
            steps {
                sh 'hadolint Dockerfile | tee hadolint_lint.txt'
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
                sh "docker rm -f ${imgname}"
                sh "docker rmi -f ${registry}/${imgname}:${params.TAG}" 
                sh "docker build -t ${registry}/${imgname}:${params.TAG} ."
            }
        }

        stage('Push image to repository'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'c14811cd-90a9-4099-b6b9-fcce7e24acb1', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh "docker login -u $USER -p $PASS"
                        sh "docker push ${registry}/${imgname}:${params.TAG}"
                    }
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
                    sh "docker run -d --name ${imgname} -p ${port}:3000 ${registry}/${imgname}:${params.TAG}"
                }
            }
        }

        stage('Vulnerability scan'){
            steps {
                script{
                    def vulnerabilities = sh(script: "trivy image --exit-code 0 --severity HIGH,MEDIUM,LOW --no-progress ${registry}/${imgname}:${params.TAG}", returnStdout: true).trim()
                    writeFile file: 'vulnerabilities.txt', text: vulnerabilities
                }
            }
        }
    }


    post {
        success {
            archiveArtifacts 'vulnerabilities.txt'
            archiveArtifacts 'hadolint_lint.txt'
        }
    }
}
