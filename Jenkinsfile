pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'python3 -m py_compile src/main.py'
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    $SONAR_SCANNER_HOME/bin/sonar-scanner
                    '''
                }
            }
        }
    }
}



