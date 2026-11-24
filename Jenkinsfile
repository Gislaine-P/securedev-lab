pipeline {
    agent any

    stages {
        stage('Construccion') {
            steps {
                sh 'docker compose build'
            }
        }
        stage('Pruebas') {
            steps {
                sh 'docker compose up -d'
                sh 'sleep 3'
                sh 'curl -sf http://127.0.0.1:8000/health | grep -q "ok"'
            }
        }
        stage('Despliegue') {
            steps {
                sh 'docker compose up -d --build'
                sh 'docker compose ps'
            }
        }
    }
}
