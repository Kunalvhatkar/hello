pipeline {
    agent any
    environment {
        PHP_SERVER = 'ip-172-31-41-126' // Update with your PHP server IP
        SSH_CREDENTIALS = 'ssh_id_rsa'  // The credential ID you added in Jenkins
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'githubtoken', url: 'https://github.com/Kunalvhatkar/hello.git'
            }
        }
        stage('Deploy to Server') {
            steps {
                sshagent(credentials: [SSH_CREDENTIALS]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no $PHP_SERVER << EOF
                    cd /var/www/html/php-app
                    git pull origin main
                    sudo systemctl restart apache2
                    EOF
                    '''
                }
            }
        }
    }
}
