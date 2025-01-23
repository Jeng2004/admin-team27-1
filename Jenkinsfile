pipeline {
    agent any
    stages {
        stage('Copy file to Docker server') {
            steps {
                sh '''
                scp -i ~/.ssh/id_rsa -r /var/lib/jenkins/workspace/team12_spering/* root@13.60.79.17:~/team12_spering
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa root@13.60.79.17 "cd ~/team12_spering && docker build -t team12_image ."
                '''
            }
        }
        stage('Create Docker Container') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa root@13.60.79.17 "
                if docker ps -a --filter 'name=team12_container' --format '{{.ID}}' | grep .; then
                    docker rm -f team12_container
                fi
                docker run -d --name team12_container -p 80:80 team12_image
                "
                '''
            }
        }
    }
}
