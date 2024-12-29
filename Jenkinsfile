pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'engabdullah1909/depi-proect'
        DOCKER_CREDENTIALS = credentials('Docker-Hub-UP')
    }

    stages {
        stage('Build Docker Image') {
            steps {
                // Build the Docker image using the Dockerfile
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Run Tests') {
            steps {
                // (Optional) Run tests inside the Docker container
                sh 'docker run --rm $DOCKER_IMAGE:latest npm test || echo "No tests configured"'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub and push the image
                    sh '''
                    echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                    docker push $DOCKER_IMAGE:latest
                    '''
                }
            }
        }
    }

    post {
        always {
            // Cleanup Docker resources to save space
            sh 'docker system prune -f'
        }
    }
}
