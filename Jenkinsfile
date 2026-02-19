pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'ls -R'
                sh 'python3 --version'
                sh 'python3 -m py_compile src/main.py'
            }
        }
    }
}
