pipeline {
  environment {
    registry = "dockpavan/nginx-build"
    registryCredential = 'dockpavan'
    DOCKER_CREDENTIALS_ID = '8893faf4-5148-4656-ac90-e80ab664b8aa'
    IMAGE_NAME = 'nginx-build-image'
    PATH = "/usr/local/bin:${env.PATH}"
    DOCKER_PATH = "/usr/local/bin:${env.PATH}"
  }
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
    timestamps()
  }
  stages {
    stage('Building image') {
      steps {
        sh 'docker version'
        script {
          def customImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
        }
      }
    }
    stage('Deploy Image') {
      steps {
        sh 'docker ps'
        script {
          docker.withRegistry("", "${env.DOCKER_CREDENTIALS_ID}") {

            // def customImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
            
            // Push the image with multiple tags
            customImage.push("${env.BUILD_ID}")
            customImage.push("latest")
          }
        }
      }
    }
  }
}
