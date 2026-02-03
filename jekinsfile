pipeline {
    agent any

    environment {
        IMAGE_NAME = "<dockerhub_username>/my_maven_app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: '<github_repo_ssh_url>', branch: 'main'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline Successful "
        }
        failure {
            echo "Pipeline Failed "
        }
        always {
            deleteDir()
        }
    }
}
