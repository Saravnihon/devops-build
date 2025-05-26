pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = '0179938f-819d-4bf4-8a98-5bd0a30c4a51'
        DOCKER_DEV_REPO = 'saravnihon/dev'
        DOCKER_PROD_REPO = 'saravnihon/prod'
    }
    stages {

        stage('Build Docker Image') {
            steps {
                script {
                        docker build -t saravnihon/dev .
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                            docker.image("${DOCKER_DEV_REPO}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh './deploy.sh'
            }
        }
    }
}
