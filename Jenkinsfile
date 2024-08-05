pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running Tests"'
            }
        }
        stage('Build') {
            steps {
                sh 'node --version' // Assuming you have a build script, modify as needed
            }
        }
    }

    post {
        always {
            junit 'test-results.xml' // Publish test results
            archiveArtifacts artifacts: '**/build/**/*.*', allowEmptyArchive: true
        }
    }
}
