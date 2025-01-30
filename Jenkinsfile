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
           
             withSonarQubeEnv(credentialsId: 'sonar') {

		sh 'mvn sonar:sonar -Dsonar.projectKey=a -Dsonar.host.url=http://192.168.2.101:9000'         
}                 
               

            }
        }
           
       
      stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            

    }tage("Build images") {
          steps {

             
              sh 'docker build -t wassimba/tpachat:$BUILD_NUMBER .'
             
             
             }
       
       
       }
  }} 
      
    
    }
}
