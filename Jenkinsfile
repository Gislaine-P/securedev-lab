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
                sh '''
                docker compose up -d
                sleep 5
                docker exec securedev-estudiante-app-1 python3 -c 'import urllib.request; data = urllib.request.urlopen("http://127.0.0.1:8000/health").read().decode(); print(data); assert "ok" in data'
                '''
            }
        }
        stage('Despliegue') {
            steps {
                sh '''
                docker compose up -d --build
                docker compose ps
                '''
            }
        }
    }
}
