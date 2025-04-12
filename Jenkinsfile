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
        SONAR_LOGIN = 'admin'
        SONAR_PASSWORD = 'Sonra@@@@@@@@2025'
    }

    stages {

          stage('Junit and Mockito Tests') {
                    steps {
                        script {
                            dir('login-service') {
                                sh 'mvn clean test'
                            }
                        }
                    }
                }

     

   
    }
}
