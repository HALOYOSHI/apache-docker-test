pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "haloyoshi/apache-test:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/HALOYOSHI/apache-docker-test.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }
        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f apache-test || true
                    docker run -d --name apache-test -p 8888:80 $DOCKER_IMAGE

                '''
            }
        }
    }
}

