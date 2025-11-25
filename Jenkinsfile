pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubCreds') // À créer dans Jenkins
        IMAGE_NAME = "rayenkorbi/student-management"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Rayen1406/Rayen_Korbi_4SIM3.git'
            }
        }

        stage('Build JAR') {
            steps {
                echo "Making mvnw executable..."
                sh 'chmod +x mvnw'

                echo "Building JAR..."
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                echo "Logging to Docker Hub..."
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"

                echo "Pushing image..."
                sh "docker push ${IMAGE_NAME}:latest"
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded! 🎉"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
