# Microservices-project-with-Country-service-for-CI-CD-pipeline
🏗️ Architecture du Pipeline CI/CD
GitHub → Jenkins → Maven → Tests → SonarQube → Nexus → Tomcat
     ↑          ↑          ↑         ↑           ↑        ↑
  Webhook    Build     Compilation  Qualité   Artefacts Déploiement

  🛠️ Technologies Utilisées
Backend
Java 21 - Langage de programmation

Spring Boot 3.5.6 - Framework d'application

Spring Data JPA - Persistance des données

Spring Data REST - API REST automatique

H2/MySQL - Bases de données

Lombok - Réduction du code boilerplate

Testing
JUnit 5 - Framework de tests

Mockito - Mocking pour les tests

Spring Boot Test - Tests d'intégration

MockMvc - Tests des contrôleurs

CI/CD
Jenkins - Serveur d'automatisation

Maven - Gestion des builds

SonarQube - Analyse de qualité du code

Nexus - Repository d'artefacts

Ansible - Automatisation du déploiement

Docker - Conteneurisation

Tomcat - Serveur d'application

ngrok - Tunnel pour les webhooks

📦 Prérequis
Système Linux Ubuntu/Debian (WSL2 pour Windows)

Java 21

Maven 3.9+

Docker

Git
