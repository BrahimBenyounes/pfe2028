
pipeline {

  tools {
        maven 'Maven'
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

  agent any

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
   stage('SonarQube Analysis') {
            steps {
                script {
                    ["contact-service", "login-service"].each { project ->
                        echo "Processing project: ${project}"
                        def projectKey = "${project}-${getTimeStamp()}"
                        dir(project) {
                            sh 'mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true'
                            sh "mvn sonar:sonar -Dsonar.login=${env.SONAR_LOGIN} -Dsonar.password=${env.SONAR_PASSWORD} -Dsonar.projectKey=${projectKey} -Dsonar.host.url=${env.SONAR_HOST_URL}"
                        }
                    }
                }
            }
        }
 

    stage('Build Docker Images') {
            steps {
                script {
                    [ "discovery-service", "gateway-service", "product-service", "formation-service","order-service", "notification-service", "login-service", "contact-service"].each { serviceName ->
                        dir(serviceName) {
                            sh "docker build -t ${serviceName}:${DOCKER_IMAGE_VERSION} ."
                        }
                    }
                }
            }
        }

        stage('Deploy Microservices') {
            steps {
                script {
                    sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down"
                    sh "docker-compose -f ${DOCKER_COMPOSE_FILE} up -d"
                }
            }
        }

        stage('Push Docker Images to Docker Hub') {
            steps {
                script {
                    sh "docker login -u ${env.DOCKER_HUB_USERNAME} -p ${env.DOCKER_HUB_PASSWORD}"
                     [ "discovery-service", "gateway-service", "product-service", "formation-service","order-service", "notification-service", "login-service", "contact-service"].each { serviceName ->
                        sh "docker tag ${serviceName}:${DOCKER_IMAGE_VERSION} ${env.DOCKER_HUB_USERNAME}/${serviceName}:${DOCKER_IMAGE_VERSION}"
                        sh "docker push ${env.DOCKER_HUB_USERNAME}/${serviceName}:${DOCKER_IMAGE_VERSION}"
                    }
                }
            }
        }
    }








}
