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
        stage('Build and sonar analysis') { 
            steps {
                withSonarQubeEnv('SonarQube') {
                sh 'mvn clean package sonar:sonar -Dsonar.host.url=http://52.77.251.65:9000 -Dsonar.login=a9a03939d6d6709b03840aeb8dcf9a9f90dfdd65' 
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
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
