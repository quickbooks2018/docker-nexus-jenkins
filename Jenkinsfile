pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages {
        stage('Build') {
            steps {
                sh script: 'mvn clean package'
                archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus') {
            steps {
                script {
                    def mavenPom = readMavenPom file: 'pom.xml'
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'app',
                            classifier: '',
                            file: "target/app-${mavenPom.version}.war",
                            type: 'war'
                        ]
                    ],
                    credentialsId: 'nexus',
                    groupId: 'cloudgeeks',
                    nexusUrl: 'nexus:8081/repository/app/',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: 'app',
                    version: "${mavenPom.version}"
                }
            }
        }
    }
}
