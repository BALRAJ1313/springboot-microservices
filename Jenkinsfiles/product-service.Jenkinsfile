pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven 3.9.6'
        DOCKERHUB_USER = 'balraj1313'
        IMAGE_NAME = 'product-service'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/BALRAJ1313/springboot-microservices.git'
            }
        }

        stage('Build') {
            steps {
                dir('product-service') {
                    bat "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                dir('product-service') {
                    bat "docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:latest ."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                        bat "docker push %DOCKERHUB_USER%/%IMAGE_NAME%:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl rollout restart deployment/product-service'
            }
        }
    }
}
