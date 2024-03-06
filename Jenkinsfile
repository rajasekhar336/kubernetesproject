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
                        // Push the Docker image with build number tag
                        app.push("${env.BUILD_NUMBER}")

                        // Try to push the image with "latest" tag
                        try {
                            sh "docker push 730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack:latest"
                        } catch (Exception e) {
                            echo "The 'latest' tag already exists in the repository and cannot be overwritten. Pushing as '${env.BUILD_NUMBER}' instead."
                            // Tag the new image as the latest build number
                            sh "docker tag 730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack:${env.BUILD_NUMBER} 730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack:latest"
                            // Push the new "latest" tag
                            sh "docker push 730335550052.dkr.ecr.ap-south-1.amazonaws.com/rajack:latest"
                        }
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

