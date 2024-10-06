pipeline {
    agent any
    stages {
        stage('Step 1') {
            steps {
                sh '''
				containers=("cafe-box" "cafe-box2" "cafe-box3")
                if [ "$(docker ps -aq -f name=$containers)" ]; then
                    docker stop $container
                    docker rm $container
                fi

                # Check if image 'cafe-img' exists
                if [ "$(docker images -q -f reference=cafe-img)" ]; then
                    docker rmi cafe-img
                fi
                '''
            }
        }
        stage('Image Build') {
            steps {
                sh '''
                docker build -t cafe-img .
                '''
            }
        }
        stage('Create Container') {
            steps {
                sh '''
                docker run --name cafe-box -p 8081:80 -d cafe-img
				docker run --name cafe-box2 -p 8082:80 -d cafe-img
				docker run --name cafe-box3 -p 8083:80 -d cafe-img
                '''
            }
        }
    }
} 