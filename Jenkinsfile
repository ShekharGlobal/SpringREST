pipeline {
    agent any
    environment {
        DOCKER_IMAGE = '110044/spring-rest-app' // Your Docker Hub repository
        DOCKER_TAG = '1.0'  // Image version/tag
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                git branch: 'master', url: 'https://github.com/ShekharGlobal/SpringREST.git'
            }
        }
        stage('Build') {
            steps {
                // Build the Maven project
                bat 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                bat 'mvn test'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                bat "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub and push the image using the provided credentials ID
                    withCredentials([usernamePassword(credentialsId: '95ef1bee-66f2-4c3b-94a1-a79e9e07a7a2', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                        bat "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                // Run your deployment script
                bat 'deploy_script.bat'
            }
        }
    }
}