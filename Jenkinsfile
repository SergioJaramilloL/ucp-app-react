pipeline {
    agent any

    tools {
        nodejs 'Node 26'
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

    // Configuración para el envío de correos al finalizar el build
    post {
        success {
            mail to: 'sergio.jaramillo@ucp.edu.co',
                 subject: "SUCCESSFUL BUILD: Job '${env.JOB_NAME}' [${env.BUILD_NUMBER}]",
                 body: "El pipeline finalizó exitosamente.\n\nPuedes revisar los detalles aquí: ${env.BUILD_URL}"
        }
        failure {
            mail to: 'sergio.jaramillo@ucp.edu.co',
                 subject: "FAILED BUILD: Job '${env.JOB_NAME}' [${env.BUILD_NUMBER}]",
                 body: "Ocurrió un error en la ejecución del pipeline.\n\nRevisa la salida de consola aquí: ${env.BUILD_URL}console"
        }
    }
}