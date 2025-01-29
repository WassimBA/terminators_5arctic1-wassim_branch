pipeline {
agent any
    stages {
        

        stage ("Git checkout "){
            steps{
        git branch: 'nabil', 
            url: 'https://github.com/hazem-soussi/terminators_5arctic1.git'
            }
        
        }
    
        
                stage("Compile Project") {
            steps {
                echo "Compile Project"
                sh 'mvn compile -DskipTests=true'
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
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage('Integration test') {
            steps {
                sh 'mvn verify -DskipUnitTests=true'
            }
        }

            stage("Quality code Test") {
            steps {
           
             sh 'mvn sonar:sonar -Dsonar.projectKey=nabil -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_33644a763436420a3fe65df3342abc8f1f9d2cfd' 
           }
       
     }
         stage('Build Image'){
        steps {
		sh ' docker build -t nabilcheki/nabilapp:$BUILD_NUMBER .'




}

}
	stage('Run OWASP ZAP Scan') {
            steps {
                sh '  docker run --rm --network bridge -u zap -v $(pwd):/zap/wrk:rw uctu/zap2docker-weekly:latest zap-baseline.py -t http://localhost:8089 -r zap_report.html'
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
    }
}
   
      
}
