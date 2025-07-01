pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-portfolio"
        CONTAINER_NAME = "flask-portfolio"
        PORT = "5000"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Run Container') {
            steps {
                echo 'Running Docker container...'
                sh "docker rm -f $CONTAINER_NAME || true"
                sh "docker run -d -p $PORT:$PORT --name $CONTAINER_NAME $IMAGE_NAME"
            }
        }
    }
}
