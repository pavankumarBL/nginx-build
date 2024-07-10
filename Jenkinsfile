pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'https://index.docker.io/v1/'
        DOCKER_CREDENTIALS_ID = '8893faf4-5148-4656-ac90-e80ab664b8aa'
        IMAGE_NAME = 'nginx-build-image'
        DOCKER_USERNAME = '' // To be populated from Jenkins credentials
        DOCKER_PASSWORD = '' // To be populated from Jenkins credentials
        PATH = "/usr/local/bin:${env.PATH}"
    }

    stages {
        stage('Prepare') {
            steps {
                script {
                    // Retrieve Docker credentials from Jenkins
                    withCredentials([usernamePassword(credentialsId: "${env.DOCKER_CREDENTIALS_ID}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        env.DOCKER_USERNAME = USERNAME
                        env.DOCKER_PASSWORD = PASSWORD
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build Docker image using shell script
                    sh """
                    docker login -u ${env.DOCKER_USERNAME} -p ${env.DOCKER_PASSWORD} ${env.DOCKER_REGISTRY}
                    docker build -t ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}:${env.BUILD_ID} .
                    """
                }
            }
        }
        
        stage('Push') {
            steps {
                script {
                    // Push Docker image using shell script
                    sh """
                    docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}:${env.BUILD_ID}
                    docker tag ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}:${env.BUILD_ID} ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}:latest
                    docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up local Docker images
                    sh """
                    docker rmi ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}:${env.BUILD_ID}
                    docker rmi ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}:latest
                    """
                }
            }
        }
    }

    post {
        always {
            script {
                // Logout from Docker registry
                sh 'docker logout ${env.DOCKER_REGISTRY}'
            }
        }
    }
}
