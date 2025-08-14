pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {

                sh 'echo "Inicializando build docker image"'
                
                script {
                    dockerapp = docker.build("mateushcds/guia-jenkins:${env.BUILD_ID}", '-f guia-pratico-jenkins/src/Dockerfile')
                }

                sh 'echo "Finalizando build docker image"'
            }
        }

        stage('Push Docker Image') {
            steps {

                sh 'echo "Inicializando push docker image"'

                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                }

                sh 'echo "Finalizando push docker image"'
            }
        }

        stage('Deploy no Kubernetes') {
            steps {
                sh 'echo "Executando o comando kubectl apply"'
            }
        }
    }
}