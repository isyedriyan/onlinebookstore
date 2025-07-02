pipeline{
    agent any

    stages{
        stage('scm'){
            steps{
                checkout scm
            }
        }
        stage('build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('nexus'){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'onlinebookstore', 
                        classifier: '', 
                        file: 'target/onlinebookstore-0.0.1-SNAPSHOT.war', 
                        type: 'war'
                        ]
                    ], 
                        credentialsId: 'nexus_credential', 
                        groupId: 'com.bookstore', 
                        nexusUrl: '54.221.130.78:8081/', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'maven-snapshots', 
                        version: '0.0.1-SNAPSHOT'
            }
        }
        stage('deploy'){
            steps{
                deploy adapters: [
                    tomcat11(
                        credentialsId: 'tomcat_cred_id', 
                        path: '', 
                        url: 'http://54.221.130.78:8082/')
                        ], 
                        contextPath: '/', 
                        war: 'target/onlinebookstore-0.0.1-SNAPSHOT.war'
            }
        }
    }
}
