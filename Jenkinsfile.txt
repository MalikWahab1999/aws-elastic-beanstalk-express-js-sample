pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
    }
    stages {
        stage('installing depedencies') {
            steps {
                sh 'npm install --save'
            }
        }
        stage('getting Snyk ready') {
            steps {
                sh 'npm install -g snyk' 
                sh 'snyk auth $SNYK_TOKEN'
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
