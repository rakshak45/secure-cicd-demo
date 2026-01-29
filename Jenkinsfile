pipeline {
    agent any

    environment {
        IMAGE_NAME = "rakshak45/secure-cicd-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t secure-cicd-demo:1.0 .'
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                sh '''
                docker run --rm \
                  -v /var/run/docker.sock:/var/run/docker.sock \
                  aquasec/trivy:latest image \
                  --exit-code 0 \
                  --severity HIGH,CRITICAL \
                  secure-cicd-demo:1.0 || true
                '''
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-token',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                      docker tag secure-cicd-demo:1.0 rakshak45/secure-cicd-demo:latest
                      echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push rakshak45/secure-cicd-demo:latest
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}
