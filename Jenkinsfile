pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/RESTcrud.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Deploy (Demo)') {
            steps {
                bat '''
                echo Starting Spring Boot app for demo...

                REM Kill existing process on port 8082 (if running)
                for /f "tokens=5" %%a in ('netstat -ano ^| findstr :8082') do taskkill /F /PID %%a

                REM Run the Spring Boot jar in background
                start "" java -jar target\\RESTcrud-0.0.1-SNAPSHOT.jar --server.port=8082

                echo Demo deployment complete! App is running at http://localhost:8082
                '''
            }
        }
    }
}
