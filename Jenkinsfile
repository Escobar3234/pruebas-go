pipeline {
    agent { label 'agent3' }

    environment {
        GO111MODULE = 'on'
        PATH = "/usr/local/go/bin:${env.PATH}" // Ajusta si Go está en otra ruta
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git url: 'https://github.com/Escobar3234/pruebas-go.git', branch: 'master'
            }
        }

        stage('Verificar Go instalado') {
            steps {
                sh 'go version'
                sh 'go env'
            }
        }

        stage('Compilar ejemplos') {
            steps {
                // Solo compila subproyectos válidos que contienen código Go
                sh '''
                    for dir in $(find . -type f -name '*.go' -exec dirname {} \\; | sort -u); do
                        echo "Compilando $dir..."
                        go build "$dir" || exit 1
                    done
                '''
            }
        }

        stage('Ejecutar pruebas') {
            steps {
                // Ejecuta solo en carpetas con archivos *_test.go
                sh '''
                    for dir in $(find . -type f -name '*_test.go' -exec dirname {} \\; | sort -u); do
                        echo "Test en $dir..."
                        go test -v "$dir" || exit 1
                    done
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completado sin errores.'
        }
        failure {
            echo 'Error en la compilación o pruebas.'
        }
    }
}
