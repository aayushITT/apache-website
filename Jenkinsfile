pipeline {
    agent any

    environment {
        APACHE_SERVER = "65.0.124.177"  // Apache server IP (EC2 or other)
        APACHE_USER = "ubuntu"  // SSH Username (e.g., ubuntu for EC2)
        WEBSITE_DIR = "/var/www/html"  // Apache document root
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/aayushITT/apache-website.git'
            }
        }

        stage('Deploy to Apache Server') {
            steps {
                sshagent (credentials: ['jenkins-ssh-key-id']) {
                    sh """
                        echo "Copying files to EC2 Apache server..."
                        scp -o StrictHostKeyChecking=no index.html ${APACHE_USER}@${APACHE_SERVER}:/home/ubuntu/
                        ssh -o StrictHostKeyChecking=no ${APACHE_USER}@${APACHE_SERVER} "sudo mv /home/ubuntu/index.html ${WEBSITE_DIR}"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful! Static website deployed."
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
