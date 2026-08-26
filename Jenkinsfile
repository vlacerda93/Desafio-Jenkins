pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Obtendo o código fonte...'
            }
        }
        stage('Build / Instalação') {
            steps {
                sh 'cd api && npm install'
            }
        }
        stage('SAST (Segurança)') {
            steps {
                sh 'cd api && npm audit || true'
            }
        }
        stage('Lint & Quality') {
            steps {
                sh 'cd api && npx eslint .'
            }
        }
        stage('Testes') {
            steps {
                sh 'cd api && npm test'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Esteira CI concluída com sucesso!'
        }
        failure {
            echo 'A esteira encontrou falhas durante a execução.'
        }
    }
}