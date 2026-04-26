pipeline {
    agent any

    environment {
        NEXUS_URL = 'http://host.docker.internal:8081'
        NEXUS_REPOSITORY = 'maven-releases'
        GROUP_ID = 'com/example'
        ARTIFACT_ID = 'springboot-helloworld'
        VERSION = '1.0.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kainesrevenge0/springboot-project.git'
            }
        }

        stage('Build JAR') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Upload JAR to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    bat '''
                    curl -v -u %NEXUS_USER%:%NEXUS_PASS% ^
                    --upload-file target\\springboot-helloworld-1.0.0.jar ^
                    %NEXUS_URL%/repository/%NEXUS_REPOSITORY%/%GROUP_ID%/%ARTIFACT_ID%/%VERSION%/%ARTIFACT_ID%-%VERSION%.jar
                    '''
                }
            }
        }
    }
}
