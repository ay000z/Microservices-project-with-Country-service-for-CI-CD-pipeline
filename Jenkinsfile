pipeline {
    agent any
    tools {
        maven 'mymaven'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ay000z/Microservices-project-with-Country-service-for-CI-CD-pipeline.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t mon-app:${BUILD_NUMBER} .'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8080:8080 mon-app:${BUILD_NUMBER}'
            }
        }
    }
}
