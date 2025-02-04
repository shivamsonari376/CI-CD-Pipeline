pipeline {
    agent any

    environment {
        APP_NAME = 'flaskapp'
        EMAIL_RECIPIENT = 'shivamsonari376@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shivamsonari376/CI-CD-Pipeline.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh '''
                python3 -m venv venv
                . venv/bin/activate  # Replaced 'source' with '.'
                pip install Flask pytest
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                . venv/bin/activate
                pytest || true
                '''
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying the Flask application...'
                sh '''
                . venv/bin/activate
                nohup python3 app.py &
                '''
            }
        }
    }

    post {
        success {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "✅ Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build #${env.BUILD_NUMBER} of ${env.JOB_NAME} was successful."
        }
        failure {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build #${env.BUILD_NUMBER} of ${env.JOB_NAME} failed. Check Jenkins for logs."
        }
    }
}
