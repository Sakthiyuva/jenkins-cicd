pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test App') {
            steps {
                sh '''
                . venv/bin/activate
                python -m py_compile app.py
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f jenkins-app || true'
                sh 'docker run -d -p 5000:5000 --name jenkins-app jenkins-app'
            }
        }
    }
}
