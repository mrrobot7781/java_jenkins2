pipeline {
    agent any

    environment {
        IMAGE_NAME = "java-hello-world"
        CONTAINER_NAME = "hello-world-instance"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                sh 'javac HelloWorld.java'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker Image...'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Docker Run & Verify') {
            steps {
                echo 'Running Container...'
                // Remove existing container if it exists to avoid naming conflicts
                sh "docker rm -f ${CONTAINER_NAME} || true"
                
                // Run the container and capture the output
                sh "docker run --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
    }

    post {
        always {
            echo 'Cleaning up container...'
            sh "docker rm -f ${CONTAINER_NAME} || true"
        }
    }
}
