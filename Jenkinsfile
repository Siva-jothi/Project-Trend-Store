pipeline {
    agent any

    stages {
        stage('Access it from GitHub repo') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}