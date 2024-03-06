pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
     }
     environment {
        AWS_REGION = 'ap-south-1'
        ECR_URL = '730335550052.dkr.ecr.ap-south-1.amazonaws.com'
        DOCKER_IMAGE_NAME = 'rajack'
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
                    withAWS(region: env.AWS_REGION, credentials: 'AWS_CRED') {
                        docker.withRegistry("https://${env.ECR_URL}", "ecr:${env.AWS_REGION}") {
                            // Tag the Docker image for ECR
                            def taggedImage = "${env.ECR_URL}/${env.DOCKER_IMAGE_NAME}:${BUILD_NUMBER}"
                            docker.image(env.DOCKER_IMAGE_NAME).tag(taggedImage)

                            // Push the Docker image to ECR
                            docker.image(taggedImage).push()
                        }
                    }
                }
            }
        }
        stage('Integrate Jenkins with EKS Cluster and Deploy') {
            steps {
                kubernetesDeploy(
                    cloud: 'my-awesome-cluster', // Name of the Kubernetes cloud provider configured in Jenkins
                    kubeconfigId: 'KUBERNETES', // ID of the Kubernetes credentials configured in Jenkins
                    yaml: 'deployment.yaml' // Path to your deployment YAML file
                )
            } 
        }
    }
}

