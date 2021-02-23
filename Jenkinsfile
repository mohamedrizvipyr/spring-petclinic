pipeline {
    
    agent any

    
environment
                {
                NEXUS_VERSION = "NEXUS 3"
                NEXUS_PROTOCOL = "http"
                NEXUS_URL = "http://65.1.91.8:8081/"
                NEXUS_REPOSITORY = "petclinic"
                NEXUS_CREDENTIAL_ID = "nexus_credentials"
                }


	 stages {
         
        stage('scm')
        {
            steps 
            {
                git credentialsId: 'jenkins', url: 'https://github.com/mohamedrizvipyr/spring-petclinic.git'
            }
        }
          stage('build')
          {
            steps
            { 
                
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
         }
       
         stage('artifacts')
         {
             steps
             {
                 archiveArtifacts artifacts: 'target/*.war'
             }
        
          }
         
       //  stage('deploye')
        // {
          //   steps
            //     {
                 //withCredentials([usernameColonPassword(credentialsId: 'tocta', variable: '1-tomcat')]) 
                 
              //   sh "curl -v -u admin:admin -T /var/lib/jenkins/workspace/petclinic_pipeline/target/petclinic.war 'http://13.127.48.14:8080/manager/text/deploy?path=/pipeline_petclinic&update=true'"
             
                // }
         // }
	

stage('publish to nexus')
        {
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
                            version: '${BUILD_NUMBER}',
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                // Lets upload the pom.xml file for additional information for Transitive dependencies
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


}
          
}




}}
