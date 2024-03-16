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
                    kubernetesDeploy(
                        configs: [
                            kubernetesDeployConfig(
                                configMap: 'deployment.yaml',
                                enableConfigSubstitution: true
                            ),
                            kubernetesDeployConfig(
                                configMap: 'service.yaml',
                                enableConfigSubstitution: true
                            )
                        ]
                    )
                }
            }
        }
    }
}
