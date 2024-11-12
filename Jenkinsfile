pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = '123' // Replace with your Docker Hub credentials ID in Jenkins
        DOCKER_IMAGE = 'thushan810/myapp' // Replace with your Docker Hub image name
    }
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Add commands to run any tests needed for your application
                sh 'echo Running tests...'
                // Example: sh 'python -m unittest discover tests'
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS_ID) {
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push()
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Commands to deploy your application (e.g., updating Kubernetes or Docker Swarm)
                    // Example: sh 'kubectl apply -f deployment.yaml'
                    sh 'echo Deploying application...'
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
