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

        stage('Run Container for Test') {
            steps {
                echo 'Running Docker container (temp for testing)...'
                bat "docker rm -f temp-test || exit 0"
                bat "docker rm -f %CONTAINER_NAME% || exit 0"
                bat "docker run -d -p %PORT%:5000 --name temp-test %IMAGE_NAME%"
                bat "ping 127.0.0.1 -n 6 > nul"
            }
        }

        stage('Test Application') {
            steps {
                echo 'Performing health check...'
                bat 'curl -s http://localhost:5000 | findstr "Portfolio" || exit /b 1'
            }
        }

        stage('Clean Up Test Container') {
            steps {
                echo 'Cleaning up test container...'
                bat "docker rm -f temp-test"
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'Pushing image to DockerHub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
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

        stage('Deploy to Render') {
            steps {
                echo 'Triggering deployment to Render...'
                withCredentials([string(credentialsId: 'RENDER_DEPLOY_HOOK', variable: 'RENDER_HOOK')]) {
                    bat "curl -X POST %RENDER_HOOK%"
                }
            }
        }
    }
}
