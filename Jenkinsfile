pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                
                    nexusArtifactUploader artifacts: 
                    [
                        [artifactId: 'app', classifier: '', file: 'target/app-1.0.1.war', type: 'war']
                        
                    ],
                     credentialsId: 'nexus', groupId: 'cloudgeeks', nexusUrl: 'nexus:8081/repository/app/',
                      nexusVersion: 'nexus3', protocol: 'http', repository: 'app',
                       version: '1.0.1'
                    }
            }
        }
    }
