pipeline {
  environment {
    registry = "dockpavan/nginx-build"
    registryCredential = 'dockpavan'
    PATH= "$PATH:/usr/local/bin"
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
      }
    }
    stage('Deploy Image') {
      steps {
        sh 'docker ps'
      }
    }
  }
}
