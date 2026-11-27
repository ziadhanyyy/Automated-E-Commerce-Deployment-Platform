pipeline {
    agent any

    stages {
        stage('Build frontend') {
            steps {
                sh 'npm ci --prefix Frontend-main/Frontend'
                sh 'npm run build --prefix Frontend-main/Frontend'
            }
        }

        stage('Test backend') {
            steps {
                sh '''
                    set -e
                    cd Backend-main/Backend
                    python3 -m venv .venv
                    .venv/bin/python -m pip install -r requirements.txt
                    .venv/bin/python -m pytest
                '''
            }
        }

        stage('Test catalog service') {
            steps {
                sh '''
                    set -e
                    cd services/catalog-service
                    python3 -m venv .venv
                    .venv/bin/python -m pip install -r requirements.txt pytest
                    .venv/bin/python -m pytest
                '''
            }
        }

        stage('Build Docker images') {
            steps {
                sh 'docker compose build'
            }
        }
    }
}