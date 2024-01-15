pipeline {
  agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
  }
  environment {
      SCANNER_HOME = tool 'sonar-scanner'
      APP_NAME = "nodejstask5"
      RELEASE = "1.0.0"
      DOCKER_USER = "abdelrhmanh21"
      DOCKER_PASS = 'dockerhub'
      IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
      IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
  }

  stages {

    stage('clean workspace') {
            steps {
                cleanWs()
            }
        }

    stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/abdelrhmanH21/nodejs-task5'
            }
        }

    stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=nodejstask5 \
                    -Dsonar.projectKey=nodejstask5'''
                }
            }
        }

    stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token'
                }
            }
        }

    stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }

}
}
