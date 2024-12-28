pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ShekharGlobal/SpringREST.git'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t spring-rest-app .'
            }
        }
        stage('Push Docker Image') {
            steps {
                bat 'docker push spring-rest-app'
            }
        }
        stage('Deploy') {
            steps {
                bat 'deploy_script.bat'
            }
        }
    }
}
