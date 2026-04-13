pipeline {
    agent any

    environment {
        NEXUS_URL = 'http://nexus:8081'
        GROUP_ID = 'com.example'
        ARTIFACT_ID = 'hw6demo'
        VERSION = '0.0.1-SNAPSHOT'
        REPOSITORY = 'maven-snapshots'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Jar') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package'
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh '''
                    curl -v -u $NEXUS_USER:$NEXUS_PASS --upload-file target/hw6demo-0.0.1-SNAPSHOT.jar \
                    $NEXUS_URL/repository/$REPOSITORY/com/example/hw6demo/0.0.1-SNAPSHOT/hw6demo-0.0.1-SNAPSHOT.jar
                    '''
                }
            }
        }
    }
}
