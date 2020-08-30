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
      stage('Unit & Integration Tests') {
        steps {
          script {
            echo "${ANDROID_HOME}"
            try {
              sh './gradlew clean test --no-daemon' //run a gradle task
            } 
            finally {
              junit '**/build/test-results/test/*.xml' //make the junit test results available in any case (success & failure)
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
