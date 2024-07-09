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
                    // List the running containers and their dependencies
                    def containerList = sh(script: 'docker ps --format "{{.ID}} {{.Names}} {{.RunningFor}}" --filter "status=running"', returnStdout: true).trim().split('\n')

                    // Identify the dependent containers
                    def dependentContainers = []
                    for (container in containerList) {
                        def containerDetails = container.split() as List
                        def containerName = containerDetails[1]
                        if (containerName.startsWith("myapp")) {
                            dependentContainers.add(containerDetails[0])
                        }
                    }

                    // Stop the dependent containers
                    sh "docker stop ${dependentContainers.join(' ')}"

                    // Remove the dependent containers
                    sh "docker rm ${dependentContainers.join(' ')}"
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
