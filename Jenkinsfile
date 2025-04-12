def getTimeStamp() {
    def date = new Date()
    return date.format('yyyyMMddHHmmss')
}

pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    environment {
        DOCKER_IMAGE_VERSION = '1.0.0'
        DOCKER_HUB_USERNAME = 'brahim2023'
        DOCKER_HUB_PASSWORD = 'Lifeisgoodbrahim@@'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        SONAR_HOST_URL = 'http://192.168.186.128:9001'
        SONAR_TOKEN = 'sqa_c3bb610452c05f15d1882a711b34657c8f2bfdd3'
    }

    stages {

        stage('Build Maven Project') {
            steps {
                dir('product-service') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                script {
                    def version = "1.0-SNAPSHOT"
                    def projectName = "product-service"

                    dir('product-service') {
                        nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: '192.168.186.128:8081',
                            groupId: 'org.tokkom',
                            version: version,
                            repository: 'maven-snapshots',
                            credentialsId: 'nexus-credentials',
                            artifacts: [
                                [
                                    artifactId: projectName,
                                    classifier: '',
                                    file: "target/${projectName}-${version}.jar",
                                    type: 'jar'
                                ]
                            ]
                        )
                    }
                }
            }
        }

    }
}
