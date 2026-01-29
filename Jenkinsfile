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
