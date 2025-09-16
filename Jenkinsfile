pipeline {
  agent any

  stages {
    stage('Verify') {
      steps {
        bat '''
          dir
          docker version
        '''
      }
    }

    stage('Build Docker Image') {
      when { expression { fileExists('Dockerfile') && readFile('Dockerfile').trim() } }
      steps {
        // Build a local tag first
        bat 'docker build -t landingpage2:ci .'
      }
    }

    stage('Push Docker Image') {
      when { expression { fileExists('Dockerfile') && readFile('Dockerfile').trim() } }
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'docker-hub-credentials',   // <- must match your Jenkins credential ID
          usernameVariable: 'DOCKER_USERNAME',
          passwordVariable: 'DOCKER_PASSWORD'
        )]) {
          bat """
            echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
            docker tag landingpage2:ci %DOCKER_USERNAME%/landingpage2:latest
            docker push %DOCKER_USERNAME%/landingpage2:latest
            docker logout
          """
        }
      }
    }
  }
}
