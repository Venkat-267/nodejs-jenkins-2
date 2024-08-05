pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'node --version' // Assuming you have a build script, modify as needed
            }
        }
    }

}
