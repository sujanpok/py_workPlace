pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sujanpok/py_workPlace.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('flask-app')
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh '''
                    docker stop flask-app || true
                    docker rm flask-app || true
                    docker run -d -p 5000:5000 --name flask-app flask-app
                    '''
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
