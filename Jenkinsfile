pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubCreds')
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
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image (linux/amd64)..."
                sh """
                   docker build \
                   --platform=linux/amd64 \
                   -t ${IMAGE_NAME}:latest .
                """
            }
        }

        stage('Docker Login & Push') {
            steps {
                echo "Logging into Docker Hub..."
                sh """
                   echo ${DOCKERHUB_CREDENTIALS_PSW} | \
                   docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                """

                echo "Pushing image to Docker Hub..."
                sh """
                   for i in 1 2 3; do
                       docker push ${IMAGE_NAME}:latest && break
                       echo "Push failed... retrying in 5 seconds (attempt \$i)"
                       sleep 5
                   done
                """
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
            echo "🎉 Pipeline succeeded!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
