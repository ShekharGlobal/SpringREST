pipeline {
    agent any

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

        stage('Deploy') {
            steps {
                // Run your deployment script
                bat 'deploy_script.bat'
            }
        }
    }
}
