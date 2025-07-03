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

        stage('Build Inventory Service') {
            steps {
                dir('inventory-service') {
                    bat "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('inventory-service') {
                    bat 'docker build -t inventory-service:latest .'
                }
            }
        }

        stage('Connect to Network') {
            steps {
                bat 'docker network connect infra_default inventory-service || echo already connected'
            }
        }

        stage('Run Container') {
            steps {
                bat '''
                    docker stop inventory-service || echo already stopped
                    docker rm inventory-service || echo already removed
                    docker run -d --name inventory-service -p 8083:8083 inventory-service:latest
                    docker network connect infra_default inventory-service || echo already connected
                '''
            }
        }
    }
}
