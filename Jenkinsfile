pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url:'https://github.com/pradhisha/sample.git',branch:'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("pradhisha/flask-ci-cd")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker stop flask-app || true && docker rm flask-app || true'
                sh 'docker run -d -p 5000:5000 --name flask-app pradhisha/flask-ci-cd:latest'
            }
        }
    }
}
