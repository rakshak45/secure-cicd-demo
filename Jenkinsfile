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
