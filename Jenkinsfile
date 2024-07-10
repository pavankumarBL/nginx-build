pipeline {
  environment {
    PATH = "/usr/local/bin:${env.PATH}"
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
          sh 'docker build . -t dockpavan/nginx-server-macos'
        }
      }
    }
    stage('Deploy Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: '8893faf4-5148-4656-ac90-e80ab664b8aa', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push dockpavan/nginx-server-macos:latest'
        }
      }
    }
  }
}
