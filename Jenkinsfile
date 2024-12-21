pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh '''
                    if [ -d "api-tests-final" ]; then
                        echo "Directory 'api-tests-final' exists. Removing it..."
                        rm -rf api-tests-final
                    fi
                    echo "Cloning the repository..."
                    git clone https://github.com/PReinis/api-tests-final.git
                    '''
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh '''
                        echo "Logging in to Docker Hub..."
                        echo $DOCKER_PASSWORD | /usr/local/bin/docker login -u $DOCKER_USERNAME --password-stdin
                        '''
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    echo "Building Docker image..."
                    /usr/local/bin/docker build -t tdlreinis/api-tests:latest ./api-tests-final
                    '''
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh '''
                    echo "Pushing Docker image to Docker Hub..."
                    /usr/local/bin/docker push tdlreinis/api-tests:latest
                    '''
                }
            }
        }
    }
}
