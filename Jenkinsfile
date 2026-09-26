pipeline {
    agent any

    // Revisa GitHub cada 2 minutos en busca de cambios
    triggers {
        pollSCM('H/2 * * * *')
    }

    tools {
        nodejs 'Node_26'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SergioJaramilloL/ucp-app-react.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'npm test -- --watchAll=false --silent > test-output.txt || true'
                sh 'cat test-output.txt'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'test-output.txt', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        success {
            echo '¡Pipeline ejecutado con éxito!'
        }
        failure {
            echo 'Pipeline fallido. Revisar logs.'
        }
    }
}