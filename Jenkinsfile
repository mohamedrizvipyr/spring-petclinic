pipeline {
    
    agent any

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
         
         stage('deploye')
         {
             steps
                 {
                 //withCredentials([usernameColonPassword(credentialsId: 'tocta', variable: '1-tomcat')]) 
                 
                 sh "curl -v -u admin:admin -T /var/lib/jenkins/workspace/petclinic_pipeline/target/petclinic.war 'http://13.127.48.14:8080/manager/text/deploy?path=/pipeline_petclinic&update=true'"
             
                 }
          }

     }      
            
    }


