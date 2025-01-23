pipeline {
    agent any
    stages {
        stage("Copy file to Docker server") {
            steps {
                script {
                    try {
                        sh "scp -r /var/lib/jenkins/workspace/admin/* root@51.20.138.54:~/admin"
                    } catch (Exception e) {
                        error("Failed to copy files to Docker server: ${e.message}")
                    }
                }
            }
        }
        
        stage("Build Docker Image") {
            steps {
                script {
                    ansiblePlaybook playbook: '/var/lib/jenkins/workspace/admin/playbooks/build.yaml'
                }
            }    
        }
        
        stage("Create Docker Container") {
            steps {
                script {
                    ansiblePlaybook playbook: '/var/lib/jenkins/workspace/admin/playbooks/deploy.yaml'
                }
            }    
        } 
    }
}
