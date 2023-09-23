pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "192.168.29.8:4001"
        DOCKER_USERNAME = "admin"
        DOCKER_PASSWORD = "Harbor12345"
        PROJECT_NAME = "demo"
        IMAGE_NAME = "next-demo_app"
        VERSION_TAG = "v1.0"
    }
    stages {
        stage('STOPPING THE CURRENTLY RUNNING CONTAINER') {
            steps {
                script {
                    catchError {
                        sh 'echo Gove@1432 | sudo -S docker stop Next_app'
                    }
                }
            }
        }

        stage('DELETING THE STOPPED CONTAINER') {
            steps {
                script {
                    catchError {
                        sh 'echo Gove@1432 | sudo -S docker rm Next_app'
                    }
                }
            }
        }

        stage('DELETING THE IMAGE OF THE CONTAINER') {
            steps {
                script {
                    catchError {
                        sh 'echo Gove@1432 | sudo -S docker rmi ${DOCKER_REGISTRY}/${HARBOR_PROJECT}/${HARBOR_REPOSITORY}:$BUILD_NUMBER'
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
                    sh "docker build -t ${IMAGE_NAME}:${VERSION_TAG} ."
                    sh "docker tag ${IMAGE_NAME}:${VERSION_TAG} ${DOCKER_REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:${VERSION_TAG}"
                    sh "docker push ${DOCKER_REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:${VERSION_TAG}"
                }
            }
        }

        stage('Pull and Manage Images') {
            steps {
                script {
                    // Pull the latest image
                    sh "docker pull ${DOCKER_REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:${VERSION_TAG}"
                }
            }
        }
        stage('Run the container'){
          steps{
            script{
              sh "docker run -d --name Next_app -p 3080:3080 ${DOCKER_REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:$BUILD_NUMBER"
            }
          }
        }
    }
}
