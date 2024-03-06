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
                    app = docker.build('rajack')
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Empty'
            }
        }
        stage('Docker push') {
            steps {
                script {
                    docker.withRegistry('https://730335550052.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:us-east-1:AWS_CRED') {
                        docker.image('rajack').push('latest')
                    }
                }
            }
        }
        stage('Integrate Jenkins with EKS Cluster and Deploy') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'my-awesome-cluster', contextName: '', credentialsId: 'KUBERNETES', namespace: 'php-app', serverUrl: 'https://26B03B3671EF237E95E7221E2633D045.gr7.ap-south-1.eks.amazonaws.com']]) {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}

stage('Docker push') {
    docker.withRegistry('https://730335550052.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:us-east-1:AWS_CRED') {
        docker.image('rajack').push('latest')
    }
}


