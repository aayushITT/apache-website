pipeline {
    agent { label 'worker' }

    environment {
        WEBSITE_DIR = "/var/www/html"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/aayushITT/apache-website.git'
            }
        }

        stage('Deploy to Apache Server') {
            steps {
                sh '''
                    echo "Removing old website files..."
                    sudo rm -rf ${WEBSITE_DIR}/*

                    echo "Copying new website files..."
                    sudo cp -r * ${WEBSITE_DIR}/

                    echo "Restarting Apache server..."
                    sudo systemctl restart apache2
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful! Static website deployed."
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
