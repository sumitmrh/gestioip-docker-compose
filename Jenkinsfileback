pipeline {
    agent any

    stages {
        stage('Git Fetch') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '9819a0bc-a2b6-4896-b7ac-277e85a064e2', url: 'https://github.com/sumitmrh/gestioip-docker-compose.git']])
            }
        }
        stage('Stop All Containers') {
            steps {
                script {
                    // List all the containers (running and stopped)
                    def allContainers = sh(script: 'sudo docker ps -a --format "{{.ID}} {{.Names}}"', returnStdout: true).trim().split('\n')

                    // Stop all the containers
                    if (!allContainers.isEmpty()) {
                        sh "sudo docker stop ${allContainers.join(' ')}"
                    }
                }
            }
        }
        stage('Delete All Containers') {
            steps {
                script {
                    // List all the containers (running and stopped)
                    def allContainers = sh(script: 'sudo docker ps -a --format "{{.ID}} {{.Names}}"', returnStdout: true).trim().split('\n')

                    // Remove all the containers
                    if (!allContainers.isEmpty()) {
                        sh "sudo docker rm ${allContainers.join(' ')}"
                    }
                }
            }
        }
        stage('Dependencies') {
            steps {
                echo "Dependendencies are being checked"
            }
        }
        stage('Docker build') {
            steps {
                sh 'sudo docker compose up -d --build'
            }
        }
    }
}
