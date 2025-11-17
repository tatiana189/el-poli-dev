pipeline {
    agent any

    stages {

        stage('Clonar repositorio') {
            steps {
                echo 'Clonando el proyecto...'
                git branch: 'main', url: 'https://github.com/tatiana189/el-poli-dev.git'
            }
        }

        stage('Construir imagen Docker del backend') {
            steps {
                echo 'Construyendo imagen...'
                sh 'docker build -t elpolidev_backend ./backend'
            }
        }

        stage('Levantar contenedores con Docker Compose') {
            steps {
                echo 'Ejecutando docker-compose...'
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
