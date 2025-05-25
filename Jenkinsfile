pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                sh 'docker build -t backend-app ./backend'
            }
        }
        stage('Build Frontend Docker Image') {
            steps {
                sh 'docker build -t frontend-app ./frontend'
            }
        }
        stage('Run Containers') {
            steps {
                sh 'docker run -d -p 5000:5000 --name backend backend-app'
                sh 'docker run -d -p 80:80 --name frontend frontend-app'
            }
        }
    }
}