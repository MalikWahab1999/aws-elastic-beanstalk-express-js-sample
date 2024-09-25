pipeline {
    agent any
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
    }
    stages {
        stage('Installing dependencies') {
            steps {
                sh 'npm install --save'
            }
        }
        stage('Getting Snyk ready') {
            steps {
                sh 'npm install -g snyk'
                sh "snyk auth $SNYK_TOKEN"
                sh 'snyk test'
            }
        }
        stage('Build the image') {
            steps {
                sh 'npm run build'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
        }
        success {
            echo 'Build made successfully'
        }
        failure {
            echo 'Build failed due to some reason'
        }
    }
}
