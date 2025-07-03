pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven 3.9.6'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/BALRAJ1313/springboot-microservices.git'
            }
        }

        stage('Build Order Service') {
            steps {
                dir('order-service') {
                    sh "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('order-service') {
                    script {
                        dockerImage = docker.build("order-service:latest")
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                      docker stop order-service || true
                      docker rm order-service || true
                      docker run -d --name order-service -p 8082:8082 order-service:latest
                    '''
                }
            }
        }
    }
}
