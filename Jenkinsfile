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
        stage('Push image') {
          steps {
            echo 'Push image is in progress'
            sh '''
            docker tag rajack:latest 730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack:latest 
            docker push 730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack:latest
            '''
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