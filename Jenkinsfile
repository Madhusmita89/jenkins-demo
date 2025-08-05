pipeline {
    agent any
    environment {
        IMAGE_NAME = 'yourdockerhubusername/my-app'
    }
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/yourusername/your-repo.git'
            }
        }
        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'npm install && npm test'
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying app (you can SSH into your server or trigger k8s, etc)"
                // sh 'docker run -d -p 80:8080 $IMAGE_NAME'
            }
        }
    }
}
