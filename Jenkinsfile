pipeline {
agent any
     environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.2.101:8081/"
        NEXUS_REPOSITORY = "test-repo"
        NEXUS_CREDENTIAL_ID = "nexus-user"
       }
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
           
             withSonarQubeEnv(credentialsId: 'sonar',installationName: 'sonar') {

		sh 'mvn sonar:sonar -Dsonar.projectKey=test -Dsonar.host.url=http://192.168.2.101:9000'         
}                 
               

            }
        }
           
       
      stage("Publish to Nexus Repository Manager") {
            steps {
		sh 'echo "done"'
                    }
                }
            

	stage("Build images") {
          steps {

             
             sh 'echo "done".'
   
           }
      }
   stage('Run OWASP ZAP Scan') {
            steps {
                sh "  docker run --rm -u root -v $(pwd):/zap/wrk:rw zaproxy/zap-stable zap-baseline.py -t http://172.17.0.1:8089 -r zap_report.html "
            }
        } 
      
    
   stage('Publish ZAP Report') {
            steps {
                archiveArtifacts artifacts: 'zap_report.html', fingerprint: true
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: 'zap_report.html',
                    reportName: 'OWASP ZAP Security Report'
                ])
            }
        }
    }}
}
