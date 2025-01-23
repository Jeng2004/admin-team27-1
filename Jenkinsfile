pipeline {
    agent any
    stages {
        stage('Copy files to Docker server') {
            steps {
                script {
                    try {
                        sh '''
                        scp -i ~/.ssh/id_rsa -r /var/lib/jenkins/workspace/admin/* jenkins@51.20.138.54:~/admin
                        '''
                    } catch (Exception e) {
                        error("Failed to copy files to Docker server: ${e.message}")
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        sh '''
                        ssh -i ~/.ssh/id_rsa jenkins@51.20.138.54 "cd ~/admin && docker build -t admin-image ."
                        '''
                    } catch (Exception e) {
                        error("Failed to build Docker image: ${e.message}")
                    }
                }
            }
        }
        stage('Create Docker Container') {
            steps {
                script {
                    try {
                        sh '''
                        ssh -i ~/.ssh/id_rsa jenkins@51.20.138.54 "
                        if docker ps -a --filter 'name=admin-container' --format '{{.ID}}' | grep .; then
                            docker rm -f admin-container
                        fi
                        docker run -d --name admin-container -p 80:80 admin-image
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
