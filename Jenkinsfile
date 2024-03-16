pipeline {
    agent any
    environment {
        dockerImageName = "almakarma237/react-app"
        registryCredential = 'dockerhub-credentials'
    }
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/almakarma237/jenkins-kubernetes-deployment.git'
            }
        }
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build dockerImageName
                }
            }
        }
        stage('Pushing Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploying React.js container to Kubernetes') {
            steps {
                script {
                    sh "sed -i 's,TEST_IMAGE_NAME,${dockerImageName}:latest,' deployment.yaml"
                    sh "kubectl apply -f deployment.yaml"
                    sh "kubectl apply -f service.yaml"
                }
            }
        }
    }
}
