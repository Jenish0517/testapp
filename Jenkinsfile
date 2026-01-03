pipeline {
    agent none

    stages {

        stage('build') {
            steps {
                echo 'compiling sysfoo app...'
                sh 'mvn compile'
            }
        }

        stage('test') {
            steps {
                echo 'running unit tests...'
                sh 'mvn clean test'
            }
        }
          stage('package') {
                    steps {
                        echo 'packaging the app...'
                        sh 'mvn package -DskipTests'
                    }
                }

    tools {
        maven 'Maven 3.9.6'
    }

    post {
        always {
            echo 'This pipeline is completed..'
        }
    }
}
