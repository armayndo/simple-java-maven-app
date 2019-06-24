pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Cloning Git') {
          steps {
            git branch: 'JenkinsfileSCM', url: 'https://github.com/armayndo/simple-java-maven-app.git'
          }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Build and sonar analysis') { 
            steps {
                withSonarQubeEnv('SonarQube') {
                sh 'mvn clean package sonar:sonar' 
                }
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
