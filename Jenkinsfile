pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'  // Change if using a different EC2 user
        EC2_HOST = '13.51.56.177' // Replace with your EC2 instance IP
        SSH_KEY = credentials('jenkins-ssh-key') // Add SSH private key in Jenkins credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/Kunalvhatkar/hello.git'
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
                    sh """
                    ssh -o StrictHostKeyChecking=no -i $SSH_KEY $EC2_USER@$EC2_HOST << 'EOF'
                    cd /var/www/html
                    sudo git pull origin main
                    sudo chown -R www-data:www-data /var/www/html
                    sudo chmod -R 755 /var/www/html
                    sudo systemctl restart apache2
                    EOF
                    """
                }
            }
        }
    }
}
