pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                git 'https://github.com/coders2345/Devops_Portal.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t flask-portfolio .'
            }
        }

        stage('Run Container') {
            steps {
                echo 'Running container...'
                sh 'docker rm -f flask-portfolio || true'
                sh 'docker run -d -p 5000:5000 --name flask-portfolio flask-portfolio'
            }
        }
    }
}
