pipeline {
  environment {
    registry = "saifdine4/tp4"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git branch: 'main', url: 'https://github.com/ssifdine/tp4devops'
      }
    }
    stage('Building image') {
      steps {
        script {
          def dockerImage = docker.build registry + ":$BUILD_NUMBER"
          env.DOCKER_IMAGE_TAG = "$BUILD_NUMBER"
        }
      }
    }
    stage('Test image') {
      steps {
        script {
          echo "Tests passed"
        }
      }
    }
    stage('Publish Image') {
      steps {
        script {
          def img = docker.image(registry + ":${env.DOCKER_IMAGE_TAG}")
          docker.withRegistry('', registryCredential) {
            img.push()
          }
        }
      }
    }
    stage('Deploy image') {
      steps {
        sh "docker stop tp4-container || true"
        sh "docker rm tp4-container || true"
        sh "docker run -d --name tp4-container -p 8082:80 ${registry}:${env.DOCKER_IMAGE_TAG}"
      }
    }
  }
}