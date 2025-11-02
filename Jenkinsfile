pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Archive') {
            steps {
                sh 'zip -r app.zip *'
                archiveArtifacts artifacts: 'app.zip', fingerprint: true
            }
        }
    }
}
