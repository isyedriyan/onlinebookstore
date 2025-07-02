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
                        nexusUrl: 'http://54.221.130.78:8081/', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'maven-releases', 
                        version: '0.0.1-SNAPSHOT'
            }
        }
        stage('deploy'){
            steps{
                deploy adapters: [
                    tomcat9(
                        credentialsId: 'Tomcat', 
                        path: '', 
                        url: 'http://3.110.172.100:8090/')
                        ], 
                        contextPath: null, 
                        war: '**/*.war'
            }
        }
    }
}
