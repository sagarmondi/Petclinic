pipeline {
    agent any
    
    tools {
        sonarQube 'Sonar' // This must match the name of the SonarQube installation in Jenkins Global Tool Configuration
    }
    
    environment {
        SONARQUBE_SERVER = 'Sonar' // SonarQube server configuration name in Jenkins
        SONAR_PROJECT_KEY = 'jenkins-sonaa' // Replace with your project key
        SONAR_PROJECT_NAME = 'jenkins-sonaa' // Replace with your project name
        SONAR_PROJECT_VERSION = '1.0' // Replace with your project version
    }
    
    stages {
        stage('SCM') {
            steps {
                checkout scm // Pulls code from the repository specified in the job configuration
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'Sonar' // Ensure 'Sonar' matches the configured SonarQube scanner in Jenkins
                    withSonarQubeEnv(SONARQUBE_SERVER) { // Set the environment for SonarQube with server name
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
