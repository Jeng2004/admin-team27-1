pipeline {
    agent any
    stages {
        stage('Copy file to Docker server') {
            steps {
                sh '''
                scp -i ~/.ssh/id_rsa -r /var/lib/jenkins/workspace/admin/* root@51.20.138.54:~/admin
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa root@51.20.138.54 "cd ~/admin && docker build -t admin_image ."
                '''
            }
        }
        stage('Create Docker Container') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa root@51.20.138.54 "
                if docker ps -a --filter 'name=admin-container' --format '{{.ID}}' | grep .; then
                    docker rm -f admin-container
                fi
                docker run -d --name admin-container -p 80:80 admin-image
                "
                '''
            }
        }
    }
}
