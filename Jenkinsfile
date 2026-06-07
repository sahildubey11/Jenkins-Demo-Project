pipeline {

    agent any

    environment {
        // 1. UPDATE THIS with your actual Docker Hub username
        IMAGE_NAME = "sahildubey11/jenkins-demo" 
    }

    stages {
        // Note: The "Clone Code" stage was removed. Jenkins handles this automatically!

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Ensure you created a credential ID named 'dockerhub-creds' in Jenkins
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                // Fixed the backslashes so they execute correctly inside the Linux container environment
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d --name myapp -p 3000:3000 ${IMAGE_NAME}:latest
                '''
            }
        }
    }
}
