pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'joyce0803/lab_flask:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[credentialsId: '9a43752f-e2de-40b6-9037-543caf420f0b', url: 'https://github.com/joyce0803/lab.git']]])
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).run("-p 8081:5000")
                }
            }
        }

        stage('Test API') {
            steps {
                script {
                    bat 'python request.py'
                }
            }
        }

        stage('Declarative: Post Actions') {
            steps {
                script {
                    // Stop and remove the Docker container
                    sh "docker stop \$(docker ps -q --filter ancestor=${DOCKER_IMAGE}) && docker rm \$(docker ps -qa --filter ancestor=${DOCKER_IMAGE})"
                }
            }
        }
    }
}
