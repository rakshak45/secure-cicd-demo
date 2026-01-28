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
