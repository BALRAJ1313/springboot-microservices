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

        stage('Build Inventory Service') {
            steps {
                dir('inventory-service') {
                    sh "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('inventory-service') {
                    script {
                        dockerImage = docker.build("inventory-service:latest")
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                      docker stop inventory-service || true
                      docker rm inventory-service || true
                      docker run -d --name inventory-service -p 8083:8083 inventory-service:latest
                    '''
                }
            }
        }
    }
}
