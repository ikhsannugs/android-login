properties([pipelineTriggers([githubPush()])]) 

pipeline {
  /* specify nodes for executing */ 
  agent  any
    stages {
      stage('Sonar Qube Scan') {
        environment {
        scannerHome = tool 'sonarscanner'
        }
        steps {
          withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonarserver')
          script {
            sh "${scannerHome}/bin/sonar-scanner" \
            -Dsonar.projectKey=android-login
          }
        }
      }
      stage('Unit & Integration Tests') {
        steps {
          script {
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
