pipeline {
    environment {
        dockerImage = ''
    }
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args  '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage ('Building jar') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage ('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }

        } 
        stage ('Building image') {
            steps {
                script {
                     def dockerImage = docker.build("mvnjavaapp/latest")
                }
            }
        }
        stage ('Deploy Image') {
            steps {
                //sh './jenkins/scripts/deliver.sh'
                script {
                    dockerImage.withRun('-p 8081:8080')
                }
           }
        }
    }
} 