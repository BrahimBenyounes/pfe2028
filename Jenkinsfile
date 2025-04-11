def getTimeStamp() {
    def date = new Date()
    return date.format('yyyyMMddHHmmss')
}

pipeline {
    agent any

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

    stages {

     
        stage('SonarQube Analysis') {
            steps {
                script {
                    ["contact-service", "login-service"].each { project ->
                        def projectKey = "${project}-${getTimeStamp()}"
                        echo "Running SonarQube analysis for ${project} as ${projectKey}"
                        dir(project) {
                            sh 'mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true'
                            sh """
                                mvn sonar:sonar \
                                    -Dsonar.login=${env.SONAR_LOGIN} \
                                    -Dsonar.password=${env.SONAR_PASSWORD} \
                                    -Dsonar.projectKey=${projectKey} \
                                    -Dsonar.host.url=${env.SONAR_HOST_URL}
                            """
                        }
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    def services = [
                        "discovery-service", "gateway-service", "product-service",
                        "formation-service", "order-service", "notification-service",
                        "login-service", "contact-service"
                    ]
                    services.each { serviceName ->
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
                    def services = [
                        "discovery-service", "gateway-service", "product-service",
                        "formation-service", "order-service", "notification-service",
                        "login-service", "contact-service"
                    ]
                    services.each { serviceName ->
                        def localTag = "${serviceName}:${DOCKER_IMAGE_VERSION}"
                        def remoteTag = "${DOCKER_HUB_USERNAME}/${serviceName}:${DOCKER_IMAGE_VERSION}"
                        sh "docker tag ${localTag} ${remoteTag}"
                        sh "docker push ${remoteTag}"
                    }
                }
            }
        }
    }
}
