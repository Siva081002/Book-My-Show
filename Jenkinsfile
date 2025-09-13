pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Siva081002/Book-My-Show.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t bookmyshow-app .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8080:8080 --name bms-container bookmyshow-app'
            }
        }
    }
}
