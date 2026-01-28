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
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                sh """
                docker run --rm \
                  -v /var/run/docker.sock:/var/run/docker.sock \
                  aquasec/trivy:latest image \
                  --exit-code 1 \
                  --severity HIGH,CRITICAL \
                  ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Run Container (Test)') {
            steps {
                sh """
                docker rm -f test-app || true
                docker run -d -p 5000:5000 --name test-app ${IMAGE_NAME}:latest
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
