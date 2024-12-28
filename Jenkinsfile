pipeline {
    agent any

    environment {
        APP_NAME = "SpringREST"
        REPO_URL = "https://github.com/ShekharGlobal/SpringREST.git"
        DOCKER_IMAGE = "shekjava/springrest" // Adjust Docker image name as needed
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository from ${REPO_URL}"
                git branch: 'master', url: "${REPO_URL}" // Adjust branch if not 'main'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh './mvnw clean package' // For Maven projects, adjusts for Gradle if needed
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh './mvnw test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' // Archive test reports
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image for ${APP_NAME}..."
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    echo "Pushing Docker image to DockerHub..."
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Replace with deployment steps: Kubernetes, SCP, etc.
                sh "kubectl apply -f k8s/deployment.yaml" // Example for Kubernetes
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for errors.'
        }
    }
}
