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

                      kubeconfig(credentialsId: 'kubeconfig', serverUrl: 'https://172.23.64.205:8443') {
                        sh 'kubectl apply -f kubernetes-deployment.yml'
                    }



                }
            }
        }
    }
}