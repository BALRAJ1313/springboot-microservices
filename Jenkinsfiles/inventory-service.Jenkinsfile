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
                    script {
                        dockerImage = docker.build("inventory-service:latest")
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
              steps {
                            bat '''
                                docker-compose -f docker-compose.yml up -d --no-recreate mysql
                                docker-compose -f docker-compose.yml up -d --build inventory-service
                            '''
                  }
             }

//         stage('Run Docker Container') {
//             steps {
//                 script {
//                     bat 'docker stop inventory-service || exit 0'
//                     bat 'docker rm inventory-service || exit 0'
//                     bat 'docker run -d --name inventory-service -p 8083:8083 inventory-service:latest'
//                 }
//             }
//         }

    }
}
