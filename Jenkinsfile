pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    try {
                        git branch: 'main', url: 'https://github.com/sujanpok/py_workPlace.git'
                    } catch (Exception e) {
                        error "Failed to checkout repository: ${e.message}"
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    withEnv(['PATH+DOCKER=/usr/local/bin']) {
                        docker.build('flask-app')
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    withEnv(['PATH+DOCKER=/usr/local/bin']) {
                        docker.image('flask-app').inside {
                            sh 'pytest'
                        }
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withEnv(['PATH+DOCKER=/usr/local/bin']) {
                        docker.image('flask-app').run('-p 3030:3030')
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                try {
                    junit 'tests/reports/*.xml'
                } catch (Exception e) {
                    echo "No test report files were found: ${e.message}"
                }
            }
        }
    }
}
