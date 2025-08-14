pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {

                sh 'echo "Inicializando build image"'
                
                script {
                    dockerapp = docker.build("mateushcds/guia-jenkins:${env.BUILD_ID}, '-f guia-pratico-jenkins/src/Dockerfile'")
                }

                sh 'echo "Finalizando build image"'
            }
        }

        stage('Push Docker Image') {
            steps {

                sh 'echo "Inicializando push image"'

                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                }

                sh 'echo "Finalizando push image"'
            }
        }

        stage('Deploy no Kubernetes') {
            steps {
                sh 'echo "Executando o comando kubectl apply"'
            }
        }
    }
}