pipeline {
    agent any
    stages {      
        stage("Copy file to Docker server") {
            steps {
                sshagent(['your-ssh-credentials-id']) {
                    sh "scp -r /var/lib/jenkins/workspace/admin-team27/* root@51.21.3.215:~/admin-team27"
                }
            }
        }
        stage("Build Docker Image") {
            steps {
                ansiblePlaybook playbook: '/var/lib/jenkins/workspace/admin-team27/playbooks/build.yaml'
            }    
        } 
        stage("Create Docker Container") {
            steps {
                ansiblePlaybook playbook: '/var/lib/jenkins/workspace/admin-team27/playbooks/deploy.yaml'
            }    
        }
    }
}
