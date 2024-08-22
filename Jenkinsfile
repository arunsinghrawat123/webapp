pipeline {
    agent any

    environment {
        // Set any environment variables required for Dependency-Check
        // For example: DEP_CHECK_HOME = 'C:\dependency-check'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from SCM
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build your project (assuming Maven here)
                sh 'mvn clean install'
            }
        }

        stage('Run Dependency-Check') {
            steps {
                script {
                    // Set up the Dependency-Check command
                    def dependencyCheckCommand = 'dependency-check --project webapp --scan . --format HTML --out dependency-check-report.html'

                    // Run OWASP Dependency-Check
                    sh dependencyCheckCommand
                }
            }
        }

        stage('Publish Report') {
            steps {
                // Archive the Dependency-Check report
                archiveArtifacts artifacts: 'dependency-check-report.html', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Clean up workspace or perform other actions
            cleanWs()
        }

        success {
            // Actions on successful build
            echo 'Build and Dependency-Check completed successfully.'
        }

        failure {
            // Actions on failed build
            echo 'Build or Dependency-Check failed.'
        }
    }
}
