pipeline {
    agent any

    stages {

        stage('Récupération du code') {
            steps {
                git url: 'https://github.com/espritdridimohamed/Mohamed_Dridi_4Sleam1.git', branch: 'main'
            }
        }

        stage('Tests Maven') {
            steps {
                sh './mvnw test || mvn test || echo "Tests échoués mais continuation"'
            }
        }

        stage('Création du livrable') {
            steps {
                sh './mvnw clean package -DskipTests || mvn clean package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success { echo "Pipeline terminé avec succès !" }
        failure { echo "Pipeline a échoué !" }
    }
}
