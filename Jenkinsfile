pipeline {
  agent any
  
  stages { 
     stage('Build docker image') {
        steps {  
            sh ' docker build -t Next_app:$BUILD_NUMBER .'
        }
     }
     stage("run docker container"){
        steps{
                sh "docker run -d --name Next-app -p 3080:3080 Next_app:$BUILD_NUMBER"
        }
     }
  }   

}  
