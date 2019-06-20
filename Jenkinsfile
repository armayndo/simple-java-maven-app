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
        stage('Build') { 
            steps {
                sh 'mvn clean package' 
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
        stage('Sonar') {
            steps {
                sh "mvn sonar:sonar -Dsonar.host.url=http://3.0.182.171:9000 -Dsonar.login=a9a03939d6d6709b03840aeb8dcf9a9f90dfdd65"
            }
        }
    }
}
