pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from GitHub
                git 'https://github.com/sujanpok/py_workPlace.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build('flask-app')
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Stop and remove any existing container
                    sh '''
                    docker stop flask-app || true
                    docker rm flask-app || true
                    '''
                    
                    // Run the new Docker container
                    docker.run('flask-app', '-d -p 5000:5000 --name flask-app')
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker resources
            sh 'docker system prune -af'
        }
    }
}
