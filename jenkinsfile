pipeline {
    agent any
    tools {
        maven 'mymaven'  // Assurez-vous que ce tool est configuré dans Jenkins
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-pwd')
        REGISTRY = "jamina2385"
    }
    stages {
        stage('Checkout code') {
            steps {
                git branch: 'master', 
                url: 'https://github.com/Jamina-ENSI/Country-service'
            }
        }
        
        stage('Build Maven') {
            steps {
                sh 'mvn clean install'
            }
            post {
                success {
                    echo 'Maven build successful!'
                }
                failure {
                    echo 'Maven build failed!'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-country-service:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-pwd') {
                        docker.image("my-country-service:${env.BUILD_NUMBER}").push()
                        docker.image("my-country-service:${env.BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'kubeconfig-file', serverUrl: '']) {
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                        
                        // Vérification du déploiement
                        sh 'kubectl get pods -n jenkins'
                        sh 'kubectl get services -n jenkins'
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline execution completed'
            cleanWs()  // Nettoyer l'espace de travail
        }
        success {
            echo 'Pipeline succeeded!'
            slackSend channel: '#deployments', message: "Deployment successful: ${env.BUILD_URL}"
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
