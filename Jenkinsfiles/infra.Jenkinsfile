pipeline {
    agent any

    stages {
        stage('Start Infra Services') {
            steps {
                bat '''
                    docker-compose -p infra -f docker-compose.yml up -d mysql mongodb
                '''
            }
        }
    }
}
