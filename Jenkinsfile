pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "nadamankai/nodejs-app"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                         // Set execute permissions for node_modules/.bin
                        sh 'chmod +x node_modules/.bin/mocha'
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {

                    sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"'
                    sh 'chmod +x ./kubectl'
                    sh 'mv ./kubectl /usr/local/bin/kubectl'

                }
            }
        }
    }
}