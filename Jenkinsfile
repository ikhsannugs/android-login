properties([pipelineTriggers([githubPush()])]) 
pipeline {
  /* specify nodes for executing */ 
  agent  any
    stages {
      stage('Sonar Qube Scan') {
        withSonarQubeEnv(credentialsId: '2', installationName: 'si sonar')
        steps {
          sh './gradlew sonarqube'
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
    post {
      always {
        deleteDir()
      }
    }
  }
}
