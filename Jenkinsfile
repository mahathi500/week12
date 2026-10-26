
pipeline {
    agent any

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                sh 'python3 -m pip install -r requirements.txt'
                sh 'python3 app.py &'
                sh 'sleep 5'
                sh 'python3 -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"

                sh "docker build -t week12app:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                sh 'docker login -u mahathinalagandla -p Mahathi@123'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"

                sh "docker tag week12app:v1 mahathinalagandla/week12-app:v1"

                sh "docker push mahathinalagandla/week12-app:v1"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes"

                sh 'kubectl apply -f deployment.yaml --validate=false'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
