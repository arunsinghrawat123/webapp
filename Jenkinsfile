pipeline {
    agent any

    environment {
        // Define any environment variables here
        // Example: MAVEN_HOME = '/usr/local/maven'
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
                // Build your project (for Maven projects)
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                // Run tests (this step is included in the build stage for Maven)
                sh 'mvn test'
            }
        }

        stage('Dependency Check') {
            steps {
                // Run OWASP Dependency-Check (adjust the path and options as needed)
                sh 'dependency-check --project MyProject --scan . --format HTML --out dependency-check-report.html'
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
            echo 'Build and analysis completed successfully.'
        }

        failure {
            // Actions on failed build
            echo 'Build or analysis failed.'
        }
    }
}
