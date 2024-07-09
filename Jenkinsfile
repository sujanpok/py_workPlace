pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sujanpok/py_workPlace.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build('flask-app')
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('flask-app').inside {
                        sh 'pytest'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    docker.image('flask-app').run('-p 3030:3030')
                }
            }
        }
    }
    post {
        always {
            junit 'tests/reports/*.xml'
        }
    }
}
