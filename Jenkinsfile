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
        stage('Seguridad_OWASP_ZAP') {
            steps {
                sh '''
                mkdir -p zap-reports
                chmod 777 zap-reports
                docker run --rm --network host \
                  -v "$WORKSPACE/zap-reports:/zap/wrk/:rw" \
                  ghcr.io/zaproxy/zaproxy:stable \
                  zap-baseline.py -t http://127.0.0.1:8000 -r zap-report.html -J zap-report.json || true
                '''
                archiveArtifacts artifacts: 'zap-reports/*', allowEmptyArchive: true
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
