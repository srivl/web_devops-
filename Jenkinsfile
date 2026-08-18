pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t webapp-devops:jenkins .'
            }
        }
        
        stage('Run Docker Container') {
            steps {
                bat 'docker rm -f webapp-container || exit 0' 
                bat 'docker run -d -p 8081:80 --name webapp-container webapp-devops:jenkins'
            }
        }
        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat 'docker push srivl/webapp-devops:jenkins'
                }
            }
    }
}