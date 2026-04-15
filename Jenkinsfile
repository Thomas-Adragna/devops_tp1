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
                sh '/var/jenkins_home/.local/bin/pytest test/test.py -v --cov=src --cov-report=xml:coverage.xml --junitxml=test-results.xml'
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

        stage('Publish to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh """
                        curl -v -u admin:$NEXUS_PASS --upload-file src/main.py http://nexus.orb.lo
                        cal/repository/tp1-releases/devops/devops_tp1/1.0.54/devops_tp1-1.0.54.py
                    """
                }
            }
        }
    }
}
