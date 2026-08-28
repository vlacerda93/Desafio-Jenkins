pipeline {
    agent any

    triggers {
        githubPush()
    }

    tools {
        nodejs 'NodeJS'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Obtendo o código fonte do repositório...'
                checkout scm
            }
        }

        stage('Build / Instalação') {
            steps {
                echo 'Instalando dependências do Node.js...'
                dir('api') {
                    sh 'npm install'
                }
            }
        }

        stage('SAST (Segurança)') {
            steps {
                echo 'Executando análise estática de segurança e auditoria de vulnerabilidades...'
                dir('api') {
                    sh 'npm audit --audit-level=high || true'
                }
            }
        }

        stage('Lint & Quality') {
            steps {
                echo 'Executando análise estática de código com ESLint...'
                dir('api') {
                    sh 'npm run lint'
                }
            }
        }

        stage('Testes') {
            steps {
                echo 'Executando suíte de testes automatizados com Jest...'
                dir('api') {
                    sh 'npm test'
                }
            }
        }
    }

    post {
        always {
            echo 'Limpando o workspace do agente...'
            cleanWs()
        }
        success {
            echo 'Esteira CI concluída com sucesso! Todos os testes e validações foram aprovados.'
        }
        failure {
            echo 'A esteira encontrou falhas durante a execução. Verifique os logs das etapas acima.'
        }
    }
}
