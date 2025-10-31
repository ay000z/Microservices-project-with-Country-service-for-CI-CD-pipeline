pipeline {
    agent any
    
    stages {
        stage('Verify Project Structure') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/ay000z/Microservices-project-with-Country-service-for-CI-CD-pipeline'
                
                script {
                    // Vérification basique sans commandes shell complexes
                    echo "Vérification de la structure du projet..."
                }
            }
        }
        
        stage('List Files') {
            steps {
                sh 'ls -la'
            }
        }
    }
}
