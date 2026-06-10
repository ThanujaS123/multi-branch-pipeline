pipeline {

    agent any

    environment {
        IMAGE_NAME = "flask-app"
        CONTAINER_NAME = "flask-container"
        DISCORD_WEBHOOK = "YOUR_DISCORD_WEBHOOK_URL"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }

        stage('Build Docker Image') {

            when {
                branch 'main'
            }

            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Deploy Docker Container') {

            when {
                branch 'main'
            }

            steps {

                sh '''
                docker rm -f ${CONTAINER_NAME} || true

                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 5000:5000 \
                ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {

        success {
            echo 'Build Successful'
        }

        failure {
            echo 'Build Failed'
        }

        always {
            echo 'Pipeline Finished'
        }
    
    }
}