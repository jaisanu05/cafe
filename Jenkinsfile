pipeline {
    agent any
    stages {
        stage('Step 1') {
            steps {
                sh '''
                if [ "$(docker ps -aq -f name=cafe-box)" ]; then
                    docker stop cafe-box cafe-box2
                    docker rm cafe-box cafe-box2
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
                '''
            }
        }
    }
} 