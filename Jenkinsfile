pipeline {
  agent any

  environment {
    // Adjust these to match your Jenkins credential IDs and registry
    DOCKERHUB_CREDENTIALS = 'dockerhub-creds'   // Jenkins credentials id (username/password)
    DOCKERHUB_USERNAME    = 'harigopal118' // optional; used in image name
    IMAGE_NAME            = "${env.DOCKERHUB_USERNAME ?: 'myuser'}/zomato" // override as needed
    IMAGE_TAG             = "${env.BUILD_NUMBER ?: 'latest'}"
  }

  stages {
    stage('Checkout') {
      steps {
        https://github.com/Hari-9390-356441/zomato.git      }
    }

    stage('Build Image') {
      steps {
        script {
          // build Docker image
          docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
        }
      }
    }

    stage('Unit / Basic Checks') {
      steps {
        // placeholder: run any lint/static checks you may add later
        echo "No unit tests configured. Add steps here if needed."
      }
    }

    stage('Push to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
            docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
            // optionally push 'latest' tag as well
            docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push('latest')
          }
        }
      }
    }

    stage('Cleanup') {
      steps {
        // remove local image to free agent space
        sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
      }
    }
  }

  post {
    success {
      echo "Build and push succeeded: ${IMAGE_NAME}:${IMAGE_TAG}"
    }
    failure {
      echo "Build failed"
    }
    always {
      // kept minimal: add notifications here (email/Slack) if needed
      echo "Pipeline finished"
    }
  }
}

