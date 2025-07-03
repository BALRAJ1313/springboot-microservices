pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven 3.9.6'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/BALRAJ1313/springboot-microservices.git'
            }
        }

        stage('Build Order Service') {
            steps {
                dir('order-service') {
                    bat "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('order-service') {
                    bat 'docker build -t order-service:latest .'
                }
            }
        }

        stage('Run Container') {
            steps {
                bat '''
                    docker stop order-service || echo already stopped
                    docker rm order-service || echo already removed
                    docker run -d --name order-service -p 8082:8082 order-service:latest
                    docker network connect infra_default order-service || echo already connected
                '''
            }
        }
    }
}
