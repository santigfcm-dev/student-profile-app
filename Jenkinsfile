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

        stage('Verify Deployment') {
            steps {
                sh '''
                    for attempt in 1 2 3 4 5 6 7 8 9 10 11 12; do
                       status=$(docker inspect --format '{{.State.Health.Status}}' student-app)
                       echo "Health check $attempt: $status"

                       if [ "$status" = "healthy" ]; then
                           exit 0
                       fi

                       sleep 5
                    done

            echo "Deployment health verification failed"
            docker logs --tail 30 student-app
            exit 1
        '''
    }
}
    }
}
