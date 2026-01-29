node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build Image') {
        sh 'docker build -t secure-cicd-demo:1.0 .'
    }

    stage('Tag Image') {
        sh 'docker tag secure-cicd-demo:1.0 rakshak45/secure-cicd-demo:latest'
    }

    stage('Push Image') {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USERNAME',
            passwordVariable: 'DOCKER_PASSWORD'
        )]) {
            sh '''
              echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
              docker push rakshak45/secure-cicd-demo:latest
            '''
        }
    }
}
