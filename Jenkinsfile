pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-static-site'
        CONTAINER_NAME = 'nginx-static-site'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/HuyZee/CICD-lab.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Stop and Remove Old Container') {
            steps {
                script {
                    sh """
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    """
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    sh 'docker run -d --name $CONTAINER_NAME -p 80:80 $IMAGE_NAME'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Site deployed successfully!'
        }
        failure {
            echo '❌ Build or deployment failed.'
        }
    }
}
