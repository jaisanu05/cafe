pipeline {
    agent any

    stages {
        stage('Delete Container & Image') {
            steps {
                sh '''
                if ["$(docker ps -aq -f name=cafe-box)" ];
                then
                docker stop cafe-box
                docker rm cafe-box
                fi
                if ["$(docker images -q -f reference=cafe-img)" ];
                then
                docker rmi cafe-img
                fi
                '''
            }
        }
         stage('Build Image') {
            steps {
                sh '''
                docker build -t cafe-img .
                '''
            }
        }
         stage('Create Container') {
            steps {
                sh '''
                docker run --name cafe-box -p 8081:80 cafe-img
                '''
            }
        }
    }
}
