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
                    script {
                        dockerImage = docker.build("product-service:latest")
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                bat 'docker-compose -f docker-compose.yml up -d --build product-service'
            }
        }
    }
}
