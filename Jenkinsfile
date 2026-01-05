pipeline {
  agent any
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
        sh '''#Truncate the GIT_COMMIT to the first 7 characters
GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)

#Set the version using Maven

mvn versions: set -DnewVersion="$GIT_SHORT_COMMIT"
mvn versions:commit'''
        archiveArtifacts '**/target/*.jar'
        sh 'mvn package -DskipTests'
      }
    }

  }
  tools {
    maven 'Maven 3.9.6'
  }
  post {
    always {
      echo 'This pipeline is completed.'
    }

  }
}