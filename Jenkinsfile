pipeline {
    agent any

    tools {
        sonarScanner 'sonar-scanner'
    }

    stages {

        stage('Build') {
            steps {
                sh 'python3 -m py_compile src/main.py'
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'sonar-scanner'
                }
            }
        }
    }
}


