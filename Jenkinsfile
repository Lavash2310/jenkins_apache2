pipeline {
    agent any

    environment {
        DEBIAN_FRONTEND = 'noninteractive'
    }

    stages {
        stage ('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Lavash2310/jenkins_apache2.git'
            }
        }
        stage ('Download Apache2') {
            steps {
                sh '''
                    echo "Downloading list of packages..."
                    sudo DEBIAN_FRONTEND=noninteractive apt-get update -y
                    echo "Installing Apache2..."
                    sudo DEBIAN_FRONTEND=noninteractive apt-get install apache2 -y
                    echo "Enable and start Apache2 service..."
                    sudo systemctl enable apache2
                    sudo systemctl start apache2
                    echo "Apache2 installation completed."
                '''
            }
        }
        stage ('Check 4xx and 5xx errors') {
            steps {
                sh '''
                    echo "Checking for 4xx and 5xx errors in Apache2 logs..."
                    sudo grep "4[0-9][0-9] "/var/log/apache2/access.log || echo "No 4xx errors found."
                    sudo grep "5[0-9][0-9] "/var/log/apache2/access.log || echo "No 5xx errors found."
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline execution failed.'
        }
    }
}