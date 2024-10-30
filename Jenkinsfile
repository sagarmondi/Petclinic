pipeline {
    agent any
    
    stages {
        stage('Fetch Code') {
            steps {
                git 'https://github.com/sagarmondi/Petclinic.git'
            }
        }
        stage('Code Analysis') {
            environment {
                scannerHome = tool 'Sonar' 
            }
            steps {
                script {
                    withSonarQubeEnv('sonar') { 
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey= jenkins-sonaa \ 
                            -Dsonar.projectName= jenkins-sonaa \ 
                            -Dsonar.projectVersion=1.0 \ 
                            -Dsonar.sources=." 
                    }
                }
            }
        }
    }
}
