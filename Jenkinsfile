properties([pipelineTriggers([githubPush()])]) 

pipeline {

  agent  any
    stages {
      stage('Code Quality Check via SonarQube') {
        steps {
          script {
          def scannerHome = tool 'sonarscanner';
            withSonarQubeEnv("sonarserver") {
            sh "${tool("sonarscanner")}/bin/sonar-scanner \
            -Dsonar.projectKey=android-login \
            -Dsonar.java.binaries=."
            }
          }
        }
      }   
      stage('Build Gradle') {
        steps {
          sh './gradlew build'
        }
      }
    }
    post {
      always {
        deleteDir()
      }
    }
  }
