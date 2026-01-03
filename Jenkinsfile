pipeline {
    agent any

    tools {
        maven 'Maven 3.9.6'
    }

    stages {

        stage('Build') {
            steps {
                echo 'Compiling sysfoo app...'
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the app...'
                sh 'mvn package -DskipTests'
            }
        }
    }

    post {
        always {
            echo 'This pipeline is completed.'
        }
    }
}
