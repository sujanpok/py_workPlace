pipeline {
    agent any
    
    environment {
        DOCKER_HOST = 'tcp://192.168.3.111:2375'  // Adjust if necessary
        DOCKER_IMAGE_NAME = 'flask-app'
        CONTAINER_NAME = 'flask-app-container'
    }
    
    tools {
        dockerTool 'myDocker'
        // Maven tool is not needed for a Flask project
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/sujanpok/py_workPlace.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    docker build -t $DOCKER_IMAGE_NAME -f Dockerfile .
                    """
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh """
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p 5000:5000 $DOCKER_IMAGE_NAME
                    """
                }
            }
        }
    }

    post {
        always {
            sh 'docker system prune -af'
        }
    }
}
