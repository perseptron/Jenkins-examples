pipeline {
    agent any
    environment {
        APP_PORT=9090
        NAME="${env.WORKSPACE}"
    }
    // Save the job name in a global variable
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B package -DskipTests'
            }
        }
        stage('Integration Test') {
            parallel {
                stage('Running Application') {
                    agent any
                    options {
                        timeout(time: 60, unit: "SECONDS")
                    }
                    steps {
                        script {
                            try {
                                dir("${NAME}/target"){
                                    sh 'java -jar contact.war'
                                }
                            } catch (Throwable e) { 
                                currentBuild.result = "success"
                            }
                        }
                    }
                }
                stage('Running Test') {
                    steps {
                        sleep(time: 30, unit: "SECONDS")
                        sh 'mvn -Dtest=RestIT test'
                    }
                }
            }
        }        
    }
}
