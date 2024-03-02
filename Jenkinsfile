pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
     }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'clone'
            }
        }
        stage('Docker build') {
            steps {
                script {
                    app =  docker.build('rajack')
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Empty'
            }
        }
        stage('Docker push to ECR') {
            steps {
                script {
                    docker.withRegistry('https://730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack', 'ecr:ap-south-1:AWS_CRED') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest") 
                    }
                }
            }
        }
    }
}

