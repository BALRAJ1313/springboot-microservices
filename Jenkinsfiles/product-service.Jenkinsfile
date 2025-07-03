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

        stage('Build Product Service') {
            steps {
                dir('product-service') {
                    bat "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('product-service') {
                    bat 'docker build -t product-service:latest .'
                }
            }
        }

        stage('Run Container') {
            steps {
                bat '''
                     docker stop product-service || echo already stopped
                     docker rm product-service || echo already removed
                     docker run -d --name product-service -p 8081:8081 product-service:latest
                     docker network connect infra_default product-service || echo already connected
                    '''
            }
        }
    }
}
