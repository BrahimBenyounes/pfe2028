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
        SONAR_TOKEN = 'sqa_c3bb610452c05f15d1882a711b34657c8f2bfdd3'  // التوكن الذي تم إنشاؤه
    }

    stages {

        // stage('SonarQube Analysis') {
        //    steps {
        //        script {
        //            ["login-service"].each { project ->
        //                echo "Processing project: ${project}"
        //                def projectKey = "${project}-${getTimeStamp()}"
        //                dir(project) {
        //                    sh 'mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true'
        //                    sh "mvn sonar:sonar -Dsonar.token=${env.SONAR_TOKEN} -Dsonar.projectKey=${projectKey} -Dsonar.host.url=${env.SONAR_HOST_URL}"
        //                }
        //            }
        //        }
        //    }
        // }

      stage('Upload Artifact to Nexus') {
    steps {
        script {
            def version = "1.0-SNAPSHOT" // أو تجيبها من pom.xml
            def projectName = "product-service"

            nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: '192.168.186.128:8081', // بدّلها بالـ Nexus IP/URL متاعك
                groupId: 'org.tokkom',
                version: version,
                repository: 'maven-snapshots', // استعمل المرايا الصحيحة حسب settings.xml
                credentialsId: 'nexus-credentials', // هذا ID لازم يكون مضاف في Jenkins (username/password متاع Nexus)
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
