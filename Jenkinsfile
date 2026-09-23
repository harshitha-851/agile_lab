pipeline {
    agent any // If 'windows-agent' label is not configured yet, 'any' will run it on the available node

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                python -m venv venv
                call venv\\Scripts\\activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                bat '''
                call venv\\Scripts\\activate
                pytest test_app.py
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline Build Passed Successfully!'
        }
        failure {
            echo '❌ Pipeline Build Failed!'
        }
    }
}
