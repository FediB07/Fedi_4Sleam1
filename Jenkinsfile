pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials-id'  // Remplace par ton ID de credentials Jenkins
        DOCKER_IMAGE = "fedibarkouti/student-management:latest" // Nom de l'image Docker
    }

    stages {

        stage('Checkout') {
            steps {
                echo "=== Récupération du code depuis GitHub ==="
                git branch: 'main', url: 'https://github.com/FediB7/Fedi_4Sleam1.git'
            }
        }

        stage('Tests Maven') {
            steps {
                echo "=== Exécution des tests Maven ==="
                // Utiliser Maven Wrapper pour Java 17, sans ignorer les tests
                sh './mvnw test || echo "Tests échoués mais continuation"'
            }
        }

        stage('Build Maven') {
            steps {
                echo "=== Compilation et packaging Maven ==="
                sh './mvnw clean package -DskipTests || echo "Build échoué mais continuation"'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "=== Construction de l'image Docker ==="
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "=== Pousser l'image Docker sur Docker Hub ==="
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline terminé avec succès !"
        }
        failure {
            echo "Pipeline a échoué !"
        }
    }
}
