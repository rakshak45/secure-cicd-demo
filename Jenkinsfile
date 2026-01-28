stage('Push Image') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh '''
              docker tag secure-cicd-demo:1.0 rakshak45/secure-cicd-demo:latest
              echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
              docker push rakshak45/secure-cicd-demo:latest
            '''
        }
    }
}
