pipeline {
    agent any

    environment {
        EC2_IP = '<your-ec2-public-ip>'
        SSH_CREDENTIALS = 'your-ssh-credentials-id'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }

      stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/keerthana-tamil/banking-backend.git'
            }
        }

       stage('Application_Build') {
        withEnv(["MVN_HOME=$mvnHome"]) {
         
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
      
      }
    }    

      stage('Application_Unit_Test') {        
        sh 'mvn compiler:testCompile surefire:test'
        step([$class: 'JUnitResultArchiver', testResults: "**/surefire-reports/*.xml"])
    } 

        stage('Application_Deploy') {
        sh 'cp $(pwd)/target/*.war /home/ubuntu/your-app.war /opt/tomcat/webapps/'
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
