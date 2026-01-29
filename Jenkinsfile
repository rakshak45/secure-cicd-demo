pipeline {
    agent any

    environment {
        IMAGE_NAME = "rakshak45/secure-cicd-demo"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
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
                  --skip-db-update \
                  secure-cicd-demo:1.0 || true
                '''
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-token',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                      docker tag secure-cicd-demo:1.0 ${IMAGE_NAME}:${IMAGE_TAG}
                      echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push ${IMAGE_NAME}:${IMAGE_TAG}
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
