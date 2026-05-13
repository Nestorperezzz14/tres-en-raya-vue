pipeline {
    agent any

    stages {
        stage('Instalación') {
            steps {
                echo 'Instalando dependencias...'
                bat 'npm install'
            }
        }
        stage('Construcción') {
            steps {
                echo 'Compilando el proyecto...'
                bat 'npm run build'
            }
        }
        stage('Pruebas') {
            steps {
                echo 'Ejecutando tests...'
                bat 'npm run test:unit'
            }
        }
    }
}