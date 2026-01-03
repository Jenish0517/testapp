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
            when { branch 'main' }

            parallel {

                stage('Jar Package') {
                    agent {
                        docker {
                            image 'maven:3.9.6-eclipse-temurin-17-alpine'
                        }
                    }
                    steps {
                        echo 'Packaging the app...'

                        sh '''
                        GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)
                        mvn versions:set -DnewVersion="$GIT_SHORT_COMMIT"
                        mvn versions:commit
                        mvn package -DskipTests
                        '''

                        archiveArtifacts artifacts: '**/target/*.jar'
                    }
                }

                stage('Docker Build & Push') {
                    agent { label 'docker' }   
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {

                                def commitHash = env.GIT_COMMIT.take(7)

                                def image = docker.build(
                                    "initcron/sysfoo:${commitHash}",
                                    "."
                                )

                                image.push()
                                image.push('latest')
                                image.push('dev')
                            }
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'This pipeline is completed.'
        }
    }
}
