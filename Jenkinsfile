pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {

                sh 'echo "Inicializando build docker image"'
                
                script {
                    dockerapp = docker.build("mateushcds/guia-jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }

                sh 'echo "Finalizando build docker image"'
            }
        }

        stage('Push Docker Image') {
            steps {

                sh 'echo "Inicializando push docker image"'

                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                    
                }

                sh 'echo "Finalizando push docker image"'
            }
        }

        stage('Deploy no Kubernetes') {
            environment {
                TAG_VERSION = "${env.BUILD_ID}"  // Tag baseada no build do Jenkins
            }
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    script {
                        def deploymentFile = readFile('./k8s/deployment.yaml')
                        deploymentFile = deploymentFile.replaceAll('\\{\\{tag\\}\\}', "${TAG_VERSION}")
                        writeFile(file: './k8s/deployment.yaml', text: deploymentFile)
                    }

                    sh 'kubectl apply -f ./k8s/deployment.yaml --kubeconfig $KUBECONFIG'
                }
            }
        }

    }
}