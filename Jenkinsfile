pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar-server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: '']], credentialsId: 'nexuspass', groupId: 'SampleWebApp', nexusUrl: 'http://107.22.20.118:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
deploy adapters: [tomcat9(credentialsId: 'tompass', path: '', url: 'http://34.204.108.94:8080/')], contextPath: 'myapp', war: '**/*.war'              
              
              
          }
            
        }
            
        }
} 
