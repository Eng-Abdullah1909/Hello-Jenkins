pipeline {
    agent {
        docker { image 'docker:stable' } // Jenkins agent with Docker support
    }
    environment {
        DOCKER_IMAGE = 'your-username/your-app' // Replace with your Docker Hub username and app name
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials-id') // Jenkins credentials ID
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'docker run --rm $DOCKER_IMAGE:latest npm test' // Replace with your test command
            }
        }
        stage('Push Docker Image') {
            steps {
                sh '''
                echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                docker push $DOCKER_IMAGE:latest
                '''
            }
        }
    }
    post {
        always {
            sh 'docker system prune -f' // Cleanup Docker resources
        }
    }
}
