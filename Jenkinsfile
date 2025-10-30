pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "ayooz/my-country-service:${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/ay000z/Microservices-project-with-Country-service-for-CI-CD-pipeline'
                
                sh '''
                    echo "📁 Structure du projet:"
                    ls -la
                    echo "🐋 Vérification Maven Wrapper:"
                    ls -la mvnw || echo "mvnw non trouvé"
                    ls -la .mvn/ || echo ".mvn/ non trouvé"
                '''
            }
        }
        
        stage('Build Maven') {
            steps {
                sh '''
                    echo "🔨 Construction avec Maven Wrapper..."
                    chmod +x mvnw  # Donner les permissions d'exécution
                    ./mvnw clean install
                    echo "✅ Build Maven terminé"
                '''
            }
        }
        
        stage('Verify Build') {
            steps {
                sh '''
                    echo "📦 Vérification du build..."
                    ls -la target/
                    find target -name "*.jar" | head -5
                    echo "✅ Fichier JAR généré"
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
                            echo "✅ Déploiement réussi!"
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
            sh """
                echo "🌐 Application disponible sur: http://localhost:30007"
                echo "📦 Image Docker: ${env.DOCKER_IMAGE}"
            """
        }
        failure {
            echo 'Pipeline failed! ❌'
        }
    }
}
