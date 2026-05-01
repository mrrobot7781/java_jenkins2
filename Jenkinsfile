pipeline {
    agent any

    environment {
        DOCKER_USER = 'mrrobot7781'
        IMAGE_NAME  = "java-hello-world"
        REGISTRY_ID = "my-docker-hub-credentials-id"
    }

    stages {
        // ... Stages for Compile, Build, and Push to DockerHub go here ...

        stage('Deploy to Minikube') {
            steps {
                echo 'Deploying to Minikube...'
                // Using the local kubeconfig directly 
                sh "kubectl apply -f deployment.yaml"
                
                echo 'Waiting for Pod to be Ready...'
                sh "kubectl rollout status deployment/java-hello-world"
            }
        }

        stage('Verify & Logs') {
            steps {
                script {
                    // Get the name of the pod created
                    def podName = sh(script: "kubectl get pods -l app=hello-world -o jsonpath='{.items[0].metadata.name}'", returnStdout: true).trim()
                    
                    echo "Checking logs for Pod: ${podName}"
                    sh "kubectl logs ${podName}"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful. Run 'minikube service java-hello-service' to access locally."
        }
    }
}
