stage('Copy files to Docker server') {
    steps {
        sshagent(['7d78dd3b-ae46-477c-927a-faf2dfc753a7']) {
            script {
                try {
                    sh '''
                    scp -o StrictHostKeyChecking=no -r /var/lib/jenkins/workspace/admin/* jenkins@51.20.138.54:~/admin
                    '''
                } catch (Exception e) {
                    echo "Error occurred: ${e.message}"
                    error("Failed to copy files to Docker server")
                }
            }
        }
    }
}
