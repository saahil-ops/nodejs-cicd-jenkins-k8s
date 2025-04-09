pipeline {
    agent any

    environment {
        IMAGE_NAME = "saahilops/nodejs-cicd-jenkins-k8s"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'git@github.com:saahil-ops/nodejs-cicd-jenkins-k8s.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh '''
    echo "DOCKER_USERNAME=$DOCKER_USERNAME"
    echo "Logging in to DockerHub..."
    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
    docker push $IMAGE_NAME:$BUILD_NUMBER
'''

                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'We will manually deploy this image to Kubernetes from command line for now.'
            }
        }
    }
}

