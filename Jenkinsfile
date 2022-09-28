pipeline{  
    environment {
    registry = "nihkumar/springapp"
    }
  agent any
  stages {
        stage('sonarverify') {
          
           steps {
                sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=capstone-project \
  -Dsonar.host.url=http://34.174.153.54:9000 \
  -Dsonar.login=sqp_aa6a99aa7cc56c4df172dc4269349ff0625c0b42\
                    -Dcheckstyle.skip'
                }
          } 
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



