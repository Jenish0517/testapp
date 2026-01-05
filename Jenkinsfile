pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Compiling sysfoo app...'
        sh 'mvn compile'
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Running unit tests...'
        sh 'mvn test'
      }
    }

    stage('Package') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Packaging the app...'
        sh '''#Truncate the GIT_COMMIT to the first 7 characters
GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)

#Set the version using Maven

mvn versions:set -DnewVersion="$GIT_SHORT_COMMIT"
mvn versions:commit'''
        sh 'mvn package -DskipTests'
        archiveArtifacts '**/target/*.jar'
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