pipeline {
    agent any
    stages {
        stage('Copy files to Docker server') {
            steps {
                sshagent(['7d78dd3b-ae46-477c-927a-faf2dfc753a7']) { 
                    script {
                        try {
                            sh '''
                            scp -r /var/lib/jenkins/workspace/admin/* jenkins@51.20.138.54:~/admin
                            '''
                        } catch (Exception e) {
                            error("Failed to copy files to Docker server: ${e.message}")
                        }
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sshagent(['7d78dd3b-ae46-477c-927a-faf2dfc753a7']) {
                    script {
                        try {
                            sh '''
                            ssh jenkins@51.20.138.54 "cd ~/admin && docker build -t admin_image ."
                            '''
                        } catch (Exception e) {
                            error("Failed to build Docker image: ${e.message}")
                        }
                    }
                }
            }
        }
        stage('Create Docker Container') {
            steps {
                sshagent(['7d78dd3b-ae46-477c-927a-faf2dfc753a7']) {
                    script {
                        try {
                            sh '''
                            ssh jenkins@51.20.138.54 "
                            if docker ps -a --filter 'name=admin-container' --format '{{.ID}}' | grep .; then
                                docker rm -f admin-container
                            fi
                            docker run -d --name admin-container -p 80:80 admin_image
                            "
                            '''
                        } catch (Exception e) {
                            error("Failed to create Docker container: ${e.message}")
                        }
                    }
                }
            }
        }
    }
}
