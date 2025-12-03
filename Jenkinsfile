pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "task2-webapp:v2"
        CONTAINER_NAME = "task2-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gowtham361/task2-webapp.git'
            }
        }

        stage('Maven Build') {
            steps {
                dir('.') {  // <- this points to the root of your repo where pom.xml is
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('.') {
                    sh "docker build -t $DOCKER_IMAGE ."
                }
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
                sh "docker run -d -p 8082:8080 --name $CONTAINER_NAME $DOCKER_IMAGE"
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment Successful!"
        }
        failure {
            echo "❌ Build/Deployment Failed! Check logs."
        }
    }
}

