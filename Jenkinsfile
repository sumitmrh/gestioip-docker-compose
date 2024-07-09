pipeline {
    agent any

    stages {
        stage('Git Fetch') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '9819a0bc-a2b6-4896-b7ac-277e85a064e2', url: 'https://github.com/sumitmrh/gestioip-docker-compose.git']])
            }
        }
        stage('Delete Dependent Containers') {
            steps {
                script {
                    // List all the containers (running and stopped)
                    def allContainers = sh(script: 'sudo docker ps -a --format "{{.ID}} {{.Names}}"', returnStdout: true).trim().split('\n')

                    // Stop the dependent containers
                    def dependentContainers = []
                    for (container in allContainers) {
                        def containerDetails = container.split() as List
                        def containerName = containerDetails[1]
                        if (containerName.startsWith("myapp")) {
                            dependentContainers.add(containerDetails[0])
                        }
                    }

                    // Stop the dependent containers
                    if (!dependentContainers.isEmpty()) {
                        sh "sudo docker stop ${dependentContainers.join(' ')}"
                    }

                    // Remove the dependent containers
                    if (!dependentContainers.isEmpty()) {
                        sh "sudo docker rm ${dependentContainers.join(' ')}"
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
