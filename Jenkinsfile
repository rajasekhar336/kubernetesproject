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
        stage('Push to ECR') {
            steps {
                script {
                    // Login to ECR
                    withAWS(region: 'your-ecr-region', credentials: 'your-aws-credentials-id') {
                        docker.withRegistry('https://your-ecr-url', 'ecr:region') {
                            // Tag the Docker image for ECR
                            docker.image('rajack').tag("your-ecr-url/rajack:${BUILD_NUMBER}")

                            // Push the Docker image to ECR
                            docker.image("your-ecr-url/rajack:${BUILD_NUMBER}").push()
                        }
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

