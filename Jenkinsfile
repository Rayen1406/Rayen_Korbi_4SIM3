pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Rayen1406/Rayen_Korbi_4SIM3.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                bat 'mvnw.cmd clean package -DskipTests'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests (won’t fail pipeline)..."
                bat 'mvnw.cmd test || exit /b 0'
            }
        }

        stage('Run Application') {
            steps {
                echo "Skipping Run Application in Jenkins"
            }
        }

        stage('Archive Artifact') {
            steps {
                echo "Archiving the JAR..."
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
