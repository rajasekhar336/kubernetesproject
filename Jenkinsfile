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
    }
}

