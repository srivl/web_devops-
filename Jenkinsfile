pipeline {
    agent any

    stages {

        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Test Kubernetes') {
            steps {
                bat 'whoami'
                bat 'echo USERPROFILE=%USERPROFILE%'
                bat 'echo KUBECONFIG=%KUBECONFIG%'
                bat 'kubectl config current-context'
                bat 'kubectl cluster-info'
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

        stage('Deploy Container') {
            steps {
                bat 'docker rm -f webapp-container || exit 0'
                bat 'docker run -d -p 8081:80 --name webapp-container srivl/webapp-devops:jenkins'
            }
        }
    }
}
