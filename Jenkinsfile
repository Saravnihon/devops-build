pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = '0179938f-819d-4bf4-8a98-5bd0a30c4a51'
        DOCKER_DEV_REPO = 'saravnihon/dev'
        DOCKER_PROD_REPO = 'saravnihon/prod'
    }
    triggers {
        githubPush()
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", credentialsId: '195f99fa-2a7d-45c9-8307-c300031aac48', url: 'https://github.com/saravnihon/devops-build.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "${env.BUILD_NUMBER}"
                    if (env.BRANCH_NAME == 'dev') {
                        docker.build("${DOCKER_DEV_REPO}:${imageTag}")
                    } else if (env.BRANCH_NAME == 'master') {
                        docker.build("${DOCKER_PROD_REPO}:${imageTag}")
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        def imageTag = "${env.BUILD_NUMBER}"
                        if (env.BRANCH_NAME == 'dev') {
                            docker.image("${DOCKER_DEV_REPO}:${imageTag}").push()
                        } else if (env.BRANCH_NAME == 'master') {
                            docker.image("${DOCKER_PROD_REPO}:${imageTag}").push()
                        }
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
