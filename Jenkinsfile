
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'bashry/dockerapp'
        CONTAINER_NAME = 'devops-web'
    }

    stages {

        stage('Test') {
            steps {
                echo 'Running application checks...'

                sh '''
                    test -f index.js
                    test -f package.json
                    test -f public/index.html

                    echo "Application files verified successfully!"
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker build \
                        -t ${DOCKER_IMAGE}:${BUILD_NUMBER} \
                        -t ${DOCKER_IMAGE}:latest .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        set -e
                        trap 'docker logout >/dev/null 2>&1 || true' EXIT

                        echo "$DOCKER_PASSWORD" | docker login \
                            -u "$DOCKER_USERNAME" --password-stdin

                        docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to EC2...'

                sh '''
                    docker pull ${DOCKER_IMAGE}:${BUILD_NUMBER}

                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true

                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        --restart unless-stopped \
                        -p 80:3000 \
                        ${DOCKER_IMAGE}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Health Check') {
            steps {
                echo 'Checking application availability...'

                sh '''
                    for i in $(seq 1 15); do
                        if curl -fsS http://127.0.0.1/ > /dev/null; then
                            echo "SUCCESS: Application is responding!"
                            exit 0
                        fi

                        echo "Waiting for application... attempt $i"
                        sleep 2
                    done

                    echo "ERROR: Application health check failed!"
                    docker logs ${CONTAINER_NAME}
                    exit 1
                '''
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning unused Docker images...'

                sh 'docker image prune -f'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: CI/CD pipeline completed!'
            echo 'Application deployed and health check passed.'
        }

        failure {
            echo 'FAILED: Pipeline encountered an error.'
            echo 'Check Jenkins Console Output.'
        }
    }
}
