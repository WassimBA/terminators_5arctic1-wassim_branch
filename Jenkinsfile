pipeline {
agent any
    stages {
        

        stage ("Git checkout "){
            steps{
        git branch: 'wassim_branch', 
            url: 'https://github.com/hazem-soussi/terminators_5arctic1.git'
            }
        
        }
    
        
                stage("Compile Project") {
            steps {
                echo "Compile Project"
                sh 'mvn compile '
            }
        }
       
        stage('Unit test') {
            steps {
                sh 'mvn test '
            }
        }
         
        
        stage("Build Project") {
            steps {
                echo "Build & test Project"
                sh 'mvn clean package '
            }
        }
        stage('Integration test') {
            steps {
                sh 'mvn verify '
            }
        }

            stage("Quality code Test") {
            steps {
           
             sh 'mvn sonar:sonar -Dsonar.projectKey=a -Dsonar.host.url=http://192.168.2.101:9000 -Dsonar.login=d6a5b4ab1830302b9a63e1e90cca5809d993af8b'         
                 
               

            }
        }
           
     stage("Publish to Nexus Repository Manager") {
            steps {
                echo "done"

}
           }
       
       stage("Build images") {
          steps {

             
              sh 'docker build -t wassimba/tpachat:$BUILD_NUMBER .'
             
             
             }
       
       
       }
       stage('Deploy our image') { 

            steps { 
               script{

                  withDockerRegistry([credentialsId:"dockerhub", url:""]){
                                      sh ' docker push wassimba/tpachat:$BUILD_NUMBER '
                   
                } 

               }}}
   
      
    
    }
}
