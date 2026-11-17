pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t student-profile-app:ci .'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker run --rm student-profile-app:ci npm test'
            }
        }
    }
}
