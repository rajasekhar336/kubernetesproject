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
        stage('Integrate Jenkins with EKS Cluster and Deploy') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'demo1', contextName: '', credentialsId: 'KUBERNETES', namespace: 'default', serverUrl: 'https://D1293696801C9405D954FF184F578FFE.gr7.ap-south-1.eks.amazonaws.com']]) {
                sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}

