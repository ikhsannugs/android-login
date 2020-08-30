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
                    try {
                        sh './gradlew clean test --no-daemon' //run a gradle task
                    } 
                    finally {
                        echo "Release Test Result"
                        junit '**app/build/test-results/testReleaseUnitTest/*.xml'
                    }
                    finally {
                        echo "Debug Test Result"
                        junit '**app/build/test-results/testDebugUnitTest/*.xml'
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
