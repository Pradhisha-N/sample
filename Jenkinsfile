pipeline {
    agent any

    environment {
        IMAGE_NAME = 'pradhisha/flask-ci-cd'
        DEPLOYMENT_NAME = 'flask-deployment'
        KUBE_CONFIG = credentials('kubeconfig-credentials') // Jenkins secret text or file credential
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Pradhisha-N/sample.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Apply Kubernetes Deployment') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-credentials', variable: 'KUBECONFIG_FILE')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG_FILE
                        kubectl apply -f k8s/deployment.yaml
                        kubectl apply -f k8s/service.yaml
                        kubectl rollout status deployment/${DEPLOYMENT_NAME}
                    '''
                }
            }
        }
    }
}
