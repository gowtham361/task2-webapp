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
                script {
                    // Build only if pom.xml exists
                    if (fileExists('pom.xml')) {
                        sh 'mvn clean package -DskipTests'
                    } else {
                        error "pom.xml not found! Check project structure."
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                """
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker run -d -p ${APP_PORT}:8080 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}
                """
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful! Visit: http://<YOUR_SERVER_IP>:${APP_PORT}/task2-webapp"
        }
        failure {
            echo "❌ Build/Deployment Failed! Check logs."
        }
    }
}


