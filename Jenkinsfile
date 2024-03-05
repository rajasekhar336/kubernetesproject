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
        stage('Pull Docker Image from ECR') {
            steps {
                script {
                    withCredentials([aws(credentialsId: 'AWS_CRED', region: 'ap-south-1')]) {
                        sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin 730335550052.dkr.ecr.${AWS_REGION}.amazonaws.com"
                        sh "docker pull 730335550052.dkr.ecr.${AWS_REGION}.amazonaws.com/:latest"
                    }
                }
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

