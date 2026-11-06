pipeline {
    agent any

    tools {
        maven 'Maven-3.10'
    }

    stages {
        stage('Check Versions') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
                sh 'docker --version'
            }
        }

        stage('Build WAR') {
            steps {
                echo 'Building TaskManagement application'
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t taskmanagement .'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying updated application'
                sh '''
                    docker rm -f taskapp || true
                    docker run -d \
                        --name taskapp \
                        --restart unless-stopped \
                        -p 8080:8080 \
                        taskmanagement
                '''
            }
        }
    }
}