pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Installing dependencies') {
            steps {
                // Install npm dependencies without Docker
                sh 'npm install --save'
            }
        }

        stage('Getting Snyk ready') {
            steps {
                // Install and configure Snyk for security checks
                sh 'npm install -g snyk'
                sh "snyk auth $SNYK_TOKEN"
                sh 'snyk test'
            }
        }

        stage('Build the image') {
            steps {
                // Build your application
                sh 'npm run build'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
        }

        success {
            echo 'Build succeeded'
        }

        failure {
            echo 'Build failed'
        }
    }
}
