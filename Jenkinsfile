pipeline{  
    environment {
    registry = "<<Your_Docker_Hub_Account/spring-demo>>"
    }
  agent any
  stages {
           stage('Build') {
          
           steps {
                
                   sh 'pwd'
                   sh 'docker build .'
                      }
       }
       
      
       stage('Publish') {
           environment {
               registryCredential = 'dockerhub'
           }
           steps{
              
              script {
                 
                  docker.withRegistry( '', registryCredential ) {
                      def appimage = docker.build registry + ":$BUILD_NUMBER"
                      appimage.push()
                      
                  }
              }
           }
       }
       stage ('Deploy') {
           steps {
               script{
                   def image_id = registry + ":$BUILD_NUMBER"
                   sh "sed -i 's|image_id|$image_id|g' deployment.yml"
                   sh "kubectl apply -f deployment.yml -f service.yml"
                   sh "kubectl rollout status deployment hello-deployment"
                   sh "kubectl get service hello-svc"
                }
           }
       }
   }
}



