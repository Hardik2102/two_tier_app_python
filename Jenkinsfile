pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                script {
                    sh 'docker build -t frontend-app ./frontend'
                }
            }
        }

        stage('Run Frontend Container') {
            steps {
                script {
                    sh '''
                    docker stop frontend || true
                    docker rm frontend || true
                    docker run -d -p 80:80 --name frontend frontend-app
                    '''
                }
            }
        }

        stage('Deploy Backend to Remote EC2') {
            steps {
                sshagent(['jenkins-backend-access']) {
                    script {
                        sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@18.206.140.74 "
                            docker stop backend || true
                            docker rm backend || true
                            rm -rf two_tier_app_python || true
                            git clone https://github.com/Hardik2102/two_tier_app_python.git
                            cd two_tier_app_python/backend
                            docker build -t backend-app .
                            docker run -d -p 5000:5000 --name backend backend-app
                        "
                        '''
                    }
                }
            }
        }
    }
}
