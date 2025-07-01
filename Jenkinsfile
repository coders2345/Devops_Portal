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
                echo '📦 Building Docker image...'
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Run Container') {
            steps {
                echo '🚀 Running Docker container...'
                bat "docker rm -f %CONTAINER_NAME% || exit 0"
                bat "docker run -d -p %PORT%:5000 --name %CONTAINER_NAME% %IMAGE_NAME%"
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo '☁️ Pushing image to DockerHub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds', // This should be set in Jenkins Credentials
                    usernameVariable: 'DOCKERHUB_USERNAME',
                    passwordVariable: 'DOCKERHUB_PASSWORD'
                )]) {
                    bat """
                        docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%
                        docker tag %IMAGE_NAME% %DOCKERHUB_USERNAME%/%IMAGE_NAME%:latest
                        docker push %DOCKERHUB_USERNAME%/%IMAGE_NAME%:latest
                    """
                }
            }
        }
    }
}
