pipeline {
    agent any
    
    tools {
        maven 'mymaven'
    }
    
    environment {
        DOCKER_IMAGE = "ayooz/my-country-service:${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/ay000z/Microservices-project-with-Country-service-for-CI-CD-pipeline'
                
                // Vérification des fichiers
                sh '''
                    echo "📁 Structure du projet:"
                    ls -la
                    echo "🐋 Dockerfile:"
                    cat Dockerfile
                '''
            }
        }
        
        stage('Build Maven') {
            steps {
                sh '''
                    echo "🔨 Démarrage de Maven..."
                    which mvn
                    mvn --version
                    echo "🔧 Construction du projet..."
                    mvn clean install -X
                '''
            }
        }
        
        stage('Verify Build') {
            steps {
                sh '''
                    echo "📦 Vérification du build..."
                    ls -la target/
                    find target -name "*.jar" | head -5
                    echo "✅ Build vérifié"
                '''
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh """
                    echo "🐋 Construction de l'image Docker..."
                    docker build -t ${env.DOCKER_IMAGE} .
                    echo "✅ Image construite:"
                    docker images | grep country-service
                """
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-token',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_TOKEN'
                    )]) {
                        sh """
                            echo "🔐 Login Docker Hub..."
                            echo \$DOCKERHUB_TOKEN | docker login -u \$DOCKERHUB_USER --password-stdin
                            echo "🚀 Pushing image..."
                            docker push ${env.DOCKER_IMAGE}
                            echo "✅ Image poussée sur Docker Hub!"
                        """
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubeconfig(credentialsId: 'kubeconfig-file', serverUrl: '') {
                        sh '''
                            echo "🎯 Déploiement Kubernetes..."
                            kubectl apply -f deployment.yaml
                            kubectl apply -f service.yaml
                            kubectl get pods -n jenkins
                        '''
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed - Build: ${env.BUILD_NUMBER}"
        }
        success {
            echo 'Pipeline succeeded! 🎉'
        }
        failure {
            echo 'Pipeline failed! ❌'
        }
    }
}
