pipeline {
    agent any

    environment {
        IMAGE_NAME = "saahiltanwar/nodejs-cicd-jenkins-k8s"
        IMAGE_TAG = "${BUILD_NUMBER}"
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
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                            echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                            docker push ${IMAGE_NAME}:${IMAGE_TAG}
                        """
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        sed 's|IMAGE_TAG|$IMAGE_TAG|g' k8s/deploy-temp.yaml > k8s/deployment-node.yaml
                        kubectl apply -f k8s/deployment-node.yaml
                        kubectl apply -f k8s/service-node.yaml
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

