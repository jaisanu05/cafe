pipeline {
    agent any
    stages {
        stage('Step 1 - Cleanup') {
            steps {
                script {
                    sh '''
                    # Stop and remove existing containers if they exist
                    for container in cafe-box cafe-box2 cafe-box3; do
                        if [ "$(docker ps -aq -f name=$container)" ]; then
                            docker stop $container
                            docker rm $container
                        fi
                    done

                    # Check if image 'cafe-img' exists and remove it
                    if [ "$(docker images -q -f reference=demo-img)" ]; then
                        docker rmi cafe-img
                    fi
                    '''
                }
            }
        }
        stage('Image Build') {
            steps {
                script {
                    sh '''
                    # Build the Docker image
                    docker build -t cafe-img .
                    '''
                }
            }
        }
        stage('Create Containers') {
            steps {
                script {
                    sh '''
                    # Run new containers from the built image
                    docker run --name cafe-box -p 8081:80 -d cafe-img
                    docker run --name cafe-box2 -p 8082:80 -d cafe-img
                    docker run --name cafe-box3 -p 8083:80 -d cafe-img
                    '''
                }
            }
        }
    }
}
