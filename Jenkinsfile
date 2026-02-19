pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'python3 -m py_compile src/main.py'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'src/__pycache__/*.pyc', fingerprint: true
            }
        }
    }
}

