pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/BrenoOtsuka/pedelogo-catalogo.git', branch: 'main'
            }
        }
        
        stage('Build Docker image') {
            steps {
                script {
                    dockerapp = docker.build(
                        "brenootsuka/pedelogo-catalogo:${env.BUILD_ID}",
                        '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
                    )
                }
            }
        }

        stage('Push Docker image to DockerHub Registry') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', 'dockerhub-credentials') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}