   pipeline {
    agent {
        docker {
            image 'python:3.9'
            args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sakthiyuva/jenkins-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test App') {
            steps {
                sh 'python -m py_compile app.py'
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
