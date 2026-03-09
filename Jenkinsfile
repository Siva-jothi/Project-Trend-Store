// deploy kubernetes cluster using jenkins pipeline for building docker image and pushing to 
// dockerhub and then deploying to kubernetes cluster using kubectl apply -f deployment.yaml and service.yaml
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sivacs2004/project-trend-store"
        DOCKERHUB_CREDENTIALS = "dockerhub-cred"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Siva-jothi/Project-Trend-Store.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'my-kubeconfig', variable: 'KUBECONFIG')]) {
                
                }
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }

    }
}