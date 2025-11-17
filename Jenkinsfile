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
                  # Bajar contenedores viejos si existen (para que no choquen los nombres)
                  docker rm -f elpolidev_backend elpolidev_db || true

                  # Levantar base de datos PostgreSQL
                  docker run -d --name elpolidev_db \
                    -e POSTGRES_USER=postgres \
                    -e POSTGRES_PASSWORD=postgres \
                    -e POSTGRES_DB=elpolidev \
                    -p 5432:5432 \
                    postgres:15

                  # Levantar backend usando la imagen recién construida
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
