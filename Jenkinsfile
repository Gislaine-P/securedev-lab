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
                sh 'sleep 5'
                sh 'docker exec securedev-estudiante-app-1 python -c "import urllib.request; data=urllib.request.urlopen(\"http://127.0.0.1:8000/health\").read().decode(); print(data); assert \"ok\" in data"'
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
