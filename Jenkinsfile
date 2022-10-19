pipeline{
    agent  any
    tools {
            maven 'maven-3.8.6'
        }
    options {
        //discardbuilds 
        buildDiscarder logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '10', daysToKeepStr: '5', numToKeepStr: '5')
        
    }
    stages{
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn --version"
                sh "mvn  install -DskipTests" //DskipTests-it skip tests in this stage
            }
        }
        stage('Test Maven - JUnit and Jacoco') {
            steps {
              sh "mvn test"
              jacoco()
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
        }
    }
}
