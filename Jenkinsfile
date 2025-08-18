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
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    script {
                        // Define a variável aqui
                        def tag_version = "${BUILD_ID}"

                        // Lê o arquivo YAML
                        def deploymentFile = readFile('./k8s/deployment.yaml')

                        // Substitui a tag
                        deploymentFile = deploymentFile.replaceAll('//{//{tag//}//}', tag_version)

                        // Salva em arquivo temporário
                        writeFile(file: './k8s/deployment-temp.yaml', text: deploymentFile)

                        // Aplica no Kubernetes
                        sh 'kubectl apply -f ./k8s/deployment-temp.yaml'
                    }
                }
            }
        }   
    }
}