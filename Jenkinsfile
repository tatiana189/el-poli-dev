pipeline {
    agent any

    stages {

        stage('Construir imagen Docker del backend') {
            steps {
                sh '''
                  cd backend
                  docker build -t elpolidev_backend .
                '''
            }
        }

        stage('Levantar contenedores con Docker Compose') {
            steps {
                sh 'docker compose -f infra/docker-compose.yml up -d'
            }
        }

        stage('Verificar contenedores') {
            steps {
                echo 'Listado de contenedores activos:'
                sh 'docker ps'
            }
        }
    }

    post {
        always {
            echo "Pipeline finalizado."
        }
    }
}
