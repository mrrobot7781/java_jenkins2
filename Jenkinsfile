pipeline {

    agent any


    environment {

        DOCKER_USER = 'mrrobot7781'

        IMAGE_NAME  = "java-hello-world"

        REGISTRY_ID = "my-docker-hub-credentials-id"

        K8S_CONFIG  = "k8s-config" // ID of the Secret File credential

    }


    stages {

        // ... Previous stages: Compile, Build, and Push remain here ...


        stage('Deploy to K8s') {

            steps {

                // Securely use the kubeconfig file

                configFileProvider([configFile(fileId: "${K8S_CONFIG}", variable: 'KUBECONFIG')]) {

                    sh "kubectl --kubeconfig=${KUBECONFIG} apply -f deployment.yaml"

                    

                    echo 'Waiting for deployment to complete...'

                    sh "kubectl --kubeconfig=${KUBECONFIG} rollout status deployment/java-hello-world"

                }

            }

        }


        stage('Verify K8s Pod') {

            steps {

                configFileProvider([configFile(fileId: "${K8S_CONFIG}", variable: 'KUBECONFIG')]) {

                    echo 'Checking Pod status...'

                    sh "kubectl --kubeconfig=${KUBECONFIG} get pods -l app=hello-world"

                    

                    echo 'Fetching Application Logs...'

                    // This grabs the logs from the pod to prove the "Hello World" ran inside K8s

                    sh "kubectl --kubeconfig=${KUBECONFIG} logs -l app=hello-world"

                }

            }

        }

    }

}
