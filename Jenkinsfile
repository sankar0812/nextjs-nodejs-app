pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "192.168.29.8:4001"
        DOCKER_USERNAME = "admin"
        DOCKER_PASSWORD = "Harbor12345"
        PROJECT_NAME = "demo"
        HARBOR_REPOSITORY = "next-demo_app"
        tagName = "latest"
    }
    stages {
        stage('STOPPING THE CURRENTLY RUNNING CONTAINER') {
            steps {
                script {
                    catchError {
                        sh 'docker stop Next_app'
                    }
                }
            }
        }

        stage('DELETING THE STOPPED CONTAINER') {
            steps {
                script {
                    catchError {
                        sh 'docker rm Next_app'
                    }
                }
            }
        }

        stage('DELETING THE IMAGE OF THE CONTAINER') {
            steps {
                script {
                    catchError {
                        sh 'docker rmi ${DOCKER_REGISTRY}/${PROJECT_NAME}/${HARBOR_REPOSITORY}:${tagName}'
                    }
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} ${DOCKER_REGISTRY}"
                }
            }
        }

        stage('Build and Push Image') {
            steps {
                script {
                    def IMAGE_NAME = "${PROJECT_NAME}/${HARBOR_REPOSITORY}"

                    sh "docker build -t ${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ."
                    sh "docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                    sh "docker rmi ${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Pull and Manage Images') {
            steps {
                script {
                    // Pull the latest image
                    sh "docker pull ${DOCKER_REGISTRY}/${PROJECT_NAME}/${HARBOR_REPOSITORY}:${BUILD_NUMBER}"
                }
            }
        }
        stage('Run the container'){
          steps{
            script{
              sh "docker run -d --name Next_app -p 3080:3080 ${DOCKER_REGISTRY}/${PROJECT_NAME}/${HARBOR_REPOSITORY}:${BUILD_NUMBER}"
            }
          }
        }
    }
}
