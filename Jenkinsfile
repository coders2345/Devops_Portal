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
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Run Container') {
            steps {
                echo 'Running Docker container...'
                bat "docker rm -f %CONTAINER_NAME% || exit 0"
                bat "docker run -d -p %PORT%:5000 --name %CONTAINER_NAME% %IMAGE_NAME%"
            }
        }
    }
}
stage('Push to DockerHub') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'Mabasha',
            passwordVariable: 'Chittu007'
        )]) {
            bat """
                docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%
                docker tag flask-portfolio %DOCKERHUB_USERNAME%/flask-portfolio:latest
                docker push %DOCKERHUB_USERNAME%/flask-portfolio:latest
            """
        }
    }
}
