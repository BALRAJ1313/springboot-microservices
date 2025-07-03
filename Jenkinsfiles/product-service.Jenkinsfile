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

        stage('Run Docker Container') {
            steps {
                script {
                    bat 'docker stop product-service || exit 0'
                    bat 'docker rm product-service || exit 0'
                    bat 'docker run -d --name product-service -p 8081:8081 product-service:latest'
                }
            }
        }
    }
}
