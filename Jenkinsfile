pipeline {
    agent {label "dev};

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'master', url: 'https://github.com/HarshShah2025/two-tier-flask-app'
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t hshah07/two-tier-flask-app:latest ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubCreds',
                                                  usernameVariable: 'DOCKERHUB_USER',
                                                  passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh "echo ${DOCKERHUB_PASS} | docker login -u ${DOCKERHUB_USER} --password-stdin"
                    sh "docker push ${DOCKERHUB_USER}/two-tier-flask-app:latest"
                }
            }
        }

        stage('Deploy with Compose') {
            steps {
               
                sh 'docker-compose down'
                sh 'docker-compose pull'
                sh 'docker-compose up -d'
            }
        }
    }
} 
