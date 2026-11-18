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
	stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f student-app 2>/dev/null || true

                    docker run -d \
                        --name student-app \
                        -p 127.0.0.1:3000:3000 \
                        --restart unless-stopped \
                        student-profile-app:ci
                '''
    }
}
    }
}
