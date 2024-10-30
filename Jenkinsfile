pipeline {
    agent any
    
    tools {
        sonar 'sonar' // Ensure 'Sonar' matches the SonarQube scanner name in Jenkins Global Tool Configuration
    }
    
    environment {
        SONARQUBE_SERVER = 'Sonar' // The SonarQube server configuration name in Jenkins
        SONAR_PROJECT_KEY = 'jenkins-sonaa' // Replace with your project key
        SONAR_PROJECT_NAME = 'jenkins-sonaa' // Replace with your project name
        SONAR_PROJECT_VERSION = '1.0' // Replace with your project version
    }
    
    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'Sonar' // Ensure 'Sonar' matches your SonarQube scanner configuration name
                    withSonarQubeEnv(SONARQUBE_SERVER) { // Sets up SonarQube environment
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                            -Dsonar.projectName=${SONAR_PROJECT_NAME} \
                            -Dsonar.projectVersion=${SONAR_PROJECT_VERSION} \
                            -Dsonar.sources=.
                        """
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
