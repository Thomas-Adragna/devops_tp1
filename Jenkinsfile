pipeline {
    agent any

    tools {
        'hudson.plugins.sonar.SonarRunnerInstallation' 'SonarScanner'
    }

    stages {

        stage('Build') {
            steps {
                sh 'python3 -m py_compile src/main.py'
            }
        }

        stage('Test') {
            steps {
                sh 'pip install pytest pytest-cov --break-system-packages'
                sh '/var/jenkins_home/.local/bin/pytest test/ --cov=src --cov-report=xml:coverage.xml --junitxml=test-results.xml'
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    script {
                        def scannerHome = tool 'SonarScanner'
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
