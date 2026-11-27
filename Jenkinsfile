pipeline {
    agent any

    stages {
        stage('Build frontend') {
            steps {
                bat 'npm ci --prefix Frontend-main\\Frontend'
                bat 'npm run build --prefix Frontend-main\\Frontend'
            }
        }

        stage('Test backend') {
            steps {
                bat '''
                    pushd Backend-main\\Backend
                    python -m venv .venv
                    if errorlevel 1 exit /b %ERRORLEVEL%
                    .venv\\Scripts\\python.exe -m pip install -r requirements.txt
                    if errorlevel 1 exit /b %ERRORLEVEL%
                    .venv\\Scripts\\python.exe -m pytest
                    set TEST_EXIT=%ERRORLEVEL%
                    popd
                    exit /b %TEST_EXIT%
                '''
            }
        }

        stage('Test catalog service') {
            steps {
                bat '''
                    pushd services\\catalog-service
                    python -m venv .venv
                    if errorlevel 1 exit /b %ERRORLEVEL%
                    .venv\\Scripts\\python.exe -m pip install -r requirements.txt pytest
                    if errorlevel 1 exit /b %ERRORLEVEL%
                    .venv\\Scripts\\python.exe -m pytest
                    set TEST_EXIT=%ERRORLEVEL%
                    popd
                    exit /b %TEST_EXIT%
                '''
            }
        }

        stage('Build Docker images') {
            steps {
                bat 'docker compose build'
            }
        }
    }
}
