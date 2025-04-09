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
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh '''
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            docker push $IMAGE_NAME:$BUILD_NUMBER
                        '''
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo "Deploying version $BUILD_NUMBER of $IMAGE_NAME to Kubernetes cluster"
                    // For now, manual deployment or shell script can go here
                    // You can replace this with kubectl apply -f deployment.yaml in future
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully! Docker Image: $IMAGE_NAME:$BUILD_NUMBER"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}

