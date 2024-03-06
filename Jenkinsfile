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
        stage('Docker push to ECR') {
            steps {
                script {
                    // Delete the 'latest' tagged image from ECR
                    sh 'aws ecr batch-delete-image --repository-name rajack --image-ids imageTag=latest --region ap-south-1'

                    // Delete the 'latest' tagged image locally
                    sh 'docker rmi rajack:latest'

                    // Push both images to ECR
                    docker.withRegistry('https://730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack', 'ecr:ap-south-1:AWS_CRED') {
                        // Push the build number tagged image
                        app.push("${env.BUILD_NUMBER}")

                        // Push the 'latest' tagged image
                        app.push("latest")
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
