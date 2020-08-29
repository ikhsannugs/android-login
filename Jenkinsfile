properties([pipelineTriggers([githubPush()])])

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'echo build'
            }
        }
        stage('Test') {
            steps {
                echo 'test'
            }
        }
        stage('Deploy') {
            when { tag "release-*" }
            steps {
                echo 'Deploying only because this commit is tagged...'
                echo 'make deploy'
            }
        }
    }
}
