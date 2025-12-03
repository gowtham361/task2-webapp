pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "task2-webapp:v2"
        CONTAINER_NAME = "task2-container"
        APP_PORT = "8082"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gowtham361/task2-webapp.git'
            }
        }

        stage('Maven Build') {
            steps {
                // No subfolder needed, just run Maven in workspace root
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh """
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                """
            }
        }

        stage('Deploy Container') {
            steps {
                sh "docker run -d -p $APP_PORT:8080 --name $CONTAINER_NAME $DOCKER_IMAGE"
            }
        }
    }

    post {
        always {
            echo '✅ Build/Deployment Finished!'
        }
        failure {
            echo '❌ Build/Deployment Failed! Check logs.'
        }
    }
}

