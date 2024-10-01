pipeline {
    agent any

    tools {
        nodejs 'Node16'  // The name of the Node.js installation you configured
    }

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
                sh 'npm install --save'
                sh 'npm audit fix'
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
                echo 'npm build' 
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
