pipeline {
    agent any

    stages {
        stage('Delete Dependent Containers') {
            steps {
                script {
                    // List the running containers and their dependencies
                    def containerList = sh(script: 'sudo docker ps --format "{{.ID}} {{.Names}} {{.RunningFor}}" --filter "status=running"', returnStdout: true).trim().split('\n')

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
                    sh "sudo docker stop ${dependentContainers.join(' ')}"

                    // Remove the dependent containers
                    sh "sudo docker rm ${dependentContainers.join(' ')}"
                }
            }
        }
        stage('Docker build') {
            steps {
                sh 'sudo docker compose up -d --build'
            }
        }
    }
}
