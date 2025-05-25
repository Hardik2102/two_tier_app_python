pipeline {
    agent any

    environment {
        FRONTEND_IP = "18.208.178.181"
        BACKEND_HOST = "ubuntu@44.211.168.136"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Hardik2102/two_tier_app_python.git'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                sh 'docker build -t frontend-app ./frontend'
            }
        }

        stage('Run Frontend Container') {
            steps {
                sh '''
                    docker stop frontend || true
                    docker rm frontend || true
                    docker run -d -p 80:80 --name frontend frontend-app
                '''
            }
        }

        stage('Deploy Backend to Remote EC2') {
            steps {
                sshagent(['backend-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no $BACKEND_HOST '
                        docker stop backend || true
                        docker rm backend || true
                        rm -rf two_tier_app_python || true
                        git clone https://github.com/Hardik2102/two_tier_app_python.git
                        cd two_tier_app_python/backend
                        docker build -t backend-app .
                        docker run -d -p 5000:5000 --name backend backend-app
                    '
                    '''
                }
            }
        }
    }
}
