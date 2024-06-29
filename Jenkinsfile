pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'todo-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/kalio007/FullStack-Todo-List-Application.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    // sh "sudo docker build -t todolist ."
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    docker run --rm ${DOCKER_IMAGE} sh -c "npm install && npm test"
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    sh '''
                    docker-compose down || true
                    docker-compose up -d --build
                    '''
                }
            }
        }
    }
        post {
                always {
                    script {
                        sh 'docker-compose down || true'
                    }
                }
                success {
                    echo 'Pipeline succeeded!'
                }
                failure {
                    echo 'Pipeline failed!'
                }
            }
}



