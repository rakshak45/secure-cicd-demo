stage('Push Image') {
    sh '''
      docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
      docker tag secure-cicd-demo:1.0 rakshak45/secure-cicd-demo:latest
      docker push rakshak45/secure-cicd-demo:latest
    '''
}
