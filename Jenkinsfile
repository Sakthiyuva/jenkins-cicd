pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        APP_NAME = 'jenkins-app'
        DOCKER_IMAGE = 'jenkins-app:latest'
    }

    stages {

        stage('Install Dependencies') {
            steps {
                echo "Creating Python virtual environment and installing dependencies"
                sh """
                    python3 -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                """
            }
        }

        stage('Test Application') {
            steps {
                echo "Running basic Python compilation test"
                sh """
                    . ${VENV_DIR}/bin/activate
                    python -m py_compile app.py
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image ${DOCKER_IMAGE}"
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Stopping old container if exists and running new container"
                sh """
                    docker rm -f ${APP_NAME} || true
                    docker run -d -p 5000:5000 --name ${APP_NAME} ${DOCKER_IMAGE}
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for errors.'
        }
    }
}
