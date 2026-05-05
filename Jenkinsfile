pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = 'lab27'
        BACKEND_PORT         = '5002'
        FRONTEND_PORT        = '3002'
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo '📥 Cloning repository...'
                git branch: 'main',
                    url: 'https://github.com/pavansai2608/Lab27.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                echo '📦 Installing backend dependencies...'
                dir('backend') {
                    bat 'npm install'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                echo '📦 Installing frontend dependencies...'
                dir('frontend') {
                    bat 'npm install'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                echo '🐳 Building Docker images...'
                bat 'docker compose build --no-cache'
            }
        }

        stage('Deploy Containers') {
            steps {
                echo '🚀 Deploying containers...'
                bat 'docker compose down --remove-orphans || exit 0'
                bat 'docker compose up -d'
            }
        }

        stage('Verify') {
            steps {
                echo '✅ Verifying running containers...'
                bat 'docker compose ps'
                bat 'docker ps --filter "name=lab27"'
            }
        }
    }

    post {
        success {
            echo '✅ lab27 Pipeline succeeded! App is live.'
            echo "🌐 Frontend : http://localhost:${env.FRONTEND_PORT}"
            echo "🔗 Backend  : http://localhost:${env.BACKEND_PORT}/api"
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above for errors.'
            bat 'docker compose logs --tail=50 || exit 0'
        }
        always {
            echo '🏁 Pipeline finished.'
        }
    }
}