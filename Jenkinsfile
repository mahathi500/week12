
pipeline {
    agent any

    stages {
        
        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                bat 'python -m pip install -r requirements.txt'
                bat 'python -m pytest -v'
            }
        }


        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"

                bat "docker build -t week12app:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                bat 'docker login -u mahathinalagandla -p Mahathi@123'
            }
        }

        stage('Pubat Docker Image to Docker Hub') {
            steps {
                echo "Pubat Docker Image to Docker Hub"

                bat "docker tag week12app:v1 mahathinalagandla/week12-app:v1"

                bat "docker pubat mahathinalagandla/week12-app:v1"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes"

                bat 'kubectl apply -f deployment.yaml --validate=false'
                bat 'kubectl apply -f service.yaml'
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
