pipeline {
    agent any

    stages {

        stage('Récupération du code') {
            steps {
                git url: 'https://github.com/espritdridimohamed/Mohamed_Dridi_4Sleam1.git', branch: 'main'
            }
        }

         stage('Test') {
            steps {
                echo "Running Maven tests..."
                sh 'mvn test'
            }
        }


        stage('Création du livrable') {
            steps {
                sh './mvnw clean package -DskipTests || echo "Échec du build mais continuation"'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success { echo "Pipeline terminé avec succès !" }
        failure { echo "Pipeline a échoué !" }
    }
}
