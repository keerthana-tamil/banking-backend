pipeline {
    agent any

    tools {
        jdk 'Java21' // Ensure JDK 21 is configured in Jenkins
        maven 'Maven3.9.7' // Ensure Maven 3.9.7 is configured in Jenkins
    }

    environment {
        EC2_IP = '<your-ec2-public-ip>'
        SSH_CREDENTIALS = 'your-ssh-credentials-id'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/keerthana-tamil/banking-backend.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: [SSH_CREDENTIALS]) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/your-app.war ubuntu@${EC2_IP}:/opt/tomcat/webapps/
                        ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} 'sudo systemctl restart tomcat'
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
