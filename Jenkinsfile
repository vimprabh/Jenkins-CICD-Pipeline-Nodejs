pipeline {
    agent any

environment {
    DOCKER_IMAGE = 'vimprabh/node-jenkins-demo'
    CONTAINER_NAME = 'node-jenkins-demo'
    APP_PORT = '3002'

    PATH = "/opt/homebrew/bin:/usr/local/bin:/Applications/Docker.app/Contents/Resources/bin:${env.HOME}/.docker/bin:${env.PATH}"
}
    stages {
        stage('Checkout') {
            steps {
                 git branch: 'main', url: 'https://github.com/AdityaGarasangi/Jenkins-CICD-Pipeline-Nodejs.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-node-app .'
            }
        }

stage('Run Docker Container') {
    steps {
        sh '''
            docker network create jenkins-network || true

            docker rm -f mongodb || true

            docker run -d \
                --name mongodb \
                --network jenkins-network \
                mongo

            docker rm -f jenkins-node-app || true

            docker run -d \
                --name jenkins-node-app \
                --network jenkins-network \
                -p 3002:3000 \
                -e MONGO_HOST=mongodb \
                -e MONGO_PORT=27017 \
                jenkins-node-app
        '''
    }
}
    }

    post {
        always {
            echo 'Cleaning up...'
           // sh 'docker rm -f jenkins-node-app || true'
        }
    }
}
