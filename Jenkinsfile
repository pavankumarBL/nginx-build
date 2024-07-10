pipeline {
  environment {
    registry = "dockpavan/nginx-build"
    registryCredential = 'dockpavan'
    
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
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps {
        sh 'docker ps'
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  }
}
