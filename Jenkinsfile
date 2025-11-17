pipeline {
    agent any

    stages {

        stage('Construir imagen Docker del backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t elpolidev_backend .'
                }
            }
        }

        stage('Levantar contenedores sin Docker Compose') {
            steps {
                sh '''
                  echo "Eliminando contenedores previos si existen..."
                  docker rm -f elpolidev_backend elpolidev_db || true

                  echo "Levantando contenedor de base de datos PostgreSQL..."
                  docker run -d --name elpolidev_db \
                    -e POSTGRES_USER=postgres \
                    -e POSTGRES_PASSWORD=postgres \
                    -e POSTGRES_DB=elpolidev \
                    postgres:15

                  echo "Levantando contenedor del backend..."
                  docker run -d --name elpolidev_backend \
                    -p 3000:3000 \
                    --link elpolidev_db:elpolidev_db \
                    elpolidev_backend
                '''
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
