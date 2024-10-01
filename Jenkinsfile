pipeline {
    agent any

    tools {
        nodejs 'Node16'  // The name of the Node.js 
    }

    environment {
        SNYK_TOKEN = credentials('snyk-api-token')  //token variable which holds the secret key
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
                sh 'npm audit fix' //fix any of the update problems in packages
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
