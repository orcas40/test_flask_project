pipeline {
    agent any

    environment {
        VIRTUALENV = "${WORKSPACE}/venv"  // Define un entorno virtual para el proyecto
    }

    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio de código desde GitHub
                git url: 'https://github.com/orcas40/test_flask_project', branch: 'main'
            }
        }
        
        stage('Set up Python Environment') {
            steps {
                // Instala virtualenv si es necesario y crea un entorno virtual
                sh '/opt/anaconda3/bin/python3 -m venv $VIRTUALENV'
                sh 'source $VIRTUALENV/bin/activate'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Activa el entorno virtual y ejecuta pip install
                sh 'source $VIRTUALENV/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // Ejecuta las pruebas con pytest
                sh 'source $VIRTUALENV/bin/activate && pytest tests/'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                // Despliega la aplicación (Ejecuta Flask en modo producción)
                sh 'source $VIRTUALENV/bin/activate && flask run --host=0.0.0.0 --port=5000'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'deactivate || true'  // Desactiva el entorno virtual
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

