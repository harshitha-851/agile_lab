pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Compiles app.py to check for syntax errors without running it
                bat 'python -m py_compile app.py'
                
                // Pause for 15 seconds to allow a second build to catch up
                sleep time: 15, unit: 'SECONDS'
                
                // Milestone ensures older builds are aborted if a newer build reaches this point first
                milestone(1)
            }
        }

        stage('Send Notification') {
            steps {
                script {
                    def recipient = 'admin@example.com'
                    def mailSubject = "Build Notification: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}"
                    def mailBody = "The build has completed. You can view the details here: ${env.BUILD_URL}"
                    
                    try {
                        // Attempting standard SMTP Mail Step
                        mail to: recipient,
                             subject: mailSubject,
                             body: mailBody
                    } catch (Exception e) {
                        // Workaround fallback if SMTP server settings are missing/unconfigured
                        echo "--- [SMTP NOT CONFIGURED - NOTIFICATION WORKAROUND] ---"
                        echo "To: ${recipient}"
                        echo "Subject: ${mailSubject}"
                        echo "Body: ${mailBody}"
                        echo "--------------------------------------------------------"
                    }
                }
            }
        }
    }
}
