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
    }
}

