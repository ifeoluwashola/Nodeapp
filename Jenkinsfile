pipeline {
    agent any
    
    environment {
        registry = "docker.io"
        registryCredential = "dockerhub-creds"
        imageName = "nodeapp"
        dockerFile = "Dockerfile"
        tag = "${env.BUILD_NUMBER}"
        repoTag = "ifeoluwashola/nodeapp:latest"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ifeoluwashola/Nodeapp.git', branch: 'main'
            }
        }
        
        stage('Build and Push') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry(registry, registryCredential) {
                        def image = docker.build(imageName + ":" + tag, "-f ${dockerFile} .")
                        image.push()
                        docker.image(imageName + ":" + tag).tag(repoTag).push()
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo "Image built and pushed to Docker Hub"
        }
        failure {
            echo "Failed to build or push the Docker image"
        }
    }
}
