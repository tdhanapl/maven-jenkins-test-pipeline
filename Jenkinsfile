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
            
        }
        stage('Sonarqube Analysis - SAST') {
            steps {
                  withSonarQubeEnv('SonarQube') {
           sh "mvn sonar:sonar \
                              -Dsonar.projectKey=maven-jenkins-pipeline \
                        -Dsonar.host.url=http://13.126.108.209:9000" 
                }
           timeout(time: 2, unit: 'MINUTES') {
                      script {
                        waitForQualityGate abortPipeline: true
                    }
                }
              }
        }
        
    }
}
