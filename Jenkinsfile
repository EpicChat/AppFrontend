pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'epic-chat/app-frontend:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'checkouting...'
                checkout scm
            }
        }

        stage('Build and test') {
            steps {
                  nodejs(nodeJSInstallationName: 'NodeJs 18') {
                    sh 'npm install'
                    sh 'npm run build'
                    sh 'npm run test'
                  }
            }
        }

        stage('Docker Build') {
            steps { 
                script {
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Run Application') {
            steps {
                script {
                    sh 'docker stop epic-chat-frontend || true'
                    sh 'docker rm epic-chat-frontend || true'

                    sh "docker run -d --name=epic-chat-frontend -p 3004:3004 --hostname=app-frontend --network=global_network $DOCKER_IMAGE"
                }
            }
        }
    }
}
