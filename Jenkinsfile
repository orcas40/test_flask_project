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
                sh '/usr/bin/python3 -m venv $VIRTUALENV'
                sh '. $VIRTUALENV/bin/activate'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Activa el entorno virtual y ejecuta pip install
                sh '. $VIRTUALENV/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // Ejecuta las pruebas con pytest
                sh '. $VIRTUALENV/bin/activate && pytest tests/'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                // Despliega la aplicación (Ejecuta Flask en modo producción)
                sh '. $VIRTUALENV/bin/activate && flask run --host=0.0.0.0 --port=5000'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            script {
            // Solo desactivar el entorno virtual si está activado
            sh '''
                if [ -n "$VIRTUAL_ENV" ]; then
                    deactivate || true
                fi
            '''
  	    }
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

