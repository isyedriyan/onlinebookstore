pipeline {
    agent any

    stages {
        stage('SCM Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Upload to Nexus') {
            steps {
                nexusArtifactUploader(
                    artifacts: [[
                        artifactId: 'onlinebookstore',
                        classifier: '',
                        file: 'target/onlinebookstore-0.0.1-SNAPSHOT.war',
                        type: 'war'
                    ]],
                    credentialsId: 'nexus_credential',
                    groupId: 'com.bookstore',
                    nexusUrl: '44.200.168.39:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: 'maven-snapshots',
                    version: '0.0.1-SNAPSHOT'
                )
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                step([
                    $class: 'DeployPublisher',
                    adapters: [[
                        $class: 'Tomcat9xAdapter',
                        credentialsId: 'tomcat_cred_id',
                        url: 'http://44.200.168.39:8082'  // <-- updated from localhost
                    ]],
                    contextPath: '/',
                    war: 'target/onlinebookstore-0.0.1-SNAPSHOT.war'
                ])
            }
        }
    }
}
