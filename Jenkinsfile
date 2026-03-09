# create a Jenkins pipeline to build and deploy the application to Kubernetes cluster. The pipeline should include stages for cloning the repository, building the Docker image, pushing it to Docker Hub, and deploying it to the Kubernetes cluster. Use appropriate Jenkins plugins and credentials management for secure handling of Docker Hub credentials.
pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Siva-jothi/Project-Trend-Store.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKERHUB_USERNAME',
                    passwordVariable: 'DOCKERHUB_PASSWORD'
                )]) {

                    sh '''
                    echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
                    docker build -t trend-store:latest .
                    docker tag trend-store:latest $DOCKERHUB_USERNAME/trend-store:latest
                    docker push $DOCKERHUB_USERNAME/trend-store:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s-deployment.yaml
                kubectl apply -f k8s-service.yaml
                '''
            }
        }

    }
}