pipeline {
    agent none
    
    stages {
        stage('Récupération du code') {
            agent any
            steps {
                echo "=== STAGE 1: Récupération du code ==="
                checkout scm
                echo "✓ Code récupéré avec succès"
            }
        }
        
        stage('Exécution des tests') {
            agent {
                docker {
                    image 'node:16' // ou 'maven:3.8', 'python:3.9', etc.
                    reuseNode true
                }
            }
            steps {
                echo "=== STAGE 2: Exécution des tests ==="
                sh 'npm install'
                sh 'npm test'
                echo "✓ Tests exécutés avec succès"
            }
        }
        
        stage('Création du bundle') {
            agent any
            steps {
                echo "=== STAGE 3: Création du bundle ==="
                sh 'npm run build'
                archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
                echo "✓ Bundle créé avec succès"
            }
        }
    }
}
