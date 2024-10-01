pipeline {
    agent {
        // Use a Docker image with Node.js and npm pre-installed
        docker {
            image 'node:16-alpine'  // You can adjust the Node.js version as needed
            args '-u root'  // Run as root to avoid permission issues
        }
    }

    environment {
        SNYK_TOKEN = credentials('snyk-api-token')  // Ensure SNYK token is stored in Jenkins credentials
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Git checkout with proper credentials if the repo is private
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/MalikWahab1999/aws-elastic-beanstalk-express-js-sample.git',
                        credentialsId: 'your-github-credentials-id'  // Use GitHub credentials
                    ]]
                ])
            }
        }

        stage('Installing dependencies') {
            steps {
                // Install npm dependencies
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
                // Build your application (replace with your build command)
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests (replace with your test command)
                sh 'npm test'
            }
        }
    }

    post {
        always {
            // Archive artifacts, like the build output
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
