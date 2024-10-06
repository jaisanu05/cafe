pipeline {
    agent any
    stages {
        stage('Step 1 - Cleanup') {
            steps {
                script {
                    sh '''
                    # Define the container names
                    containers=("cafe-box" "cafe-box2" "cafe-box3")

                    # Stop and remove existing containers if they exist
                    for container in "${containers[@]}"; do
                        if [ "$(docker ps -aq -f name=$container)" ]; then
                            docker stop $container
                            docker rm $container
                        fi
                    done

                    # Check if image 'cafe-img' exists and remove it
                    if [ "$(docker images -q cafe-img 2> /dev/null)" ]; then
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
