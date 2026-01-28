pipeline {
    agent any

    environment {
        IMAGE_NAME = "rakshak45/secure-cicd-demo"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t :latest .'
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL :latest'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhub-creds",
                    usernameVariable: "DOCKER_USER",
                    passwordVariable: "DOCKER_PASS"
                )]) {
                    sh '''
                    echo  | docker login -u  --password-stdin
                    docker push :latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
}
