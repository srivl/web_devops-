pipeline {
    agent any

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Test Kubernetes') {
            steps {
                bat '''
                    set KUBECONFIG=C:\\Users\\srikanth\\.kube\\config
                    kubectl config current-context
                    kubectl cluster-info
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t srivl/webapp-devops:jenkins .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push srivl/webapp-devops:jenkins'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                    set KUBECONFIG=C:\\Users\\srikanth\\.kube\\config

                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml

                    kubectl rollout status deployment/webapp-deployment -n deployment
                '''
            }
        }
    }
}