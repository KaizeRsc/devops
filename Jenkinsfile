pipeline {
    agent any

    tools {
        maven 'Maven' // Assurez-vous que Maven est configuré dans les outils globaux de Jenkins
    }

    environment {
        SONARQUBE_SCANNER_HOME = tool 'SonarQube Scanner'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // Récupère le code source du repository configuré dans Jenkins
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // Exécute Maven pour nettoyer et construire le package
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test' // Exécute les tests Maven
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') { // Utilisez ici le nom que vous avez donné à la configuration SonarQube dans Jenkins
                        sh "${env.SONARQUBE_SCANNER_HOME}/bin/sonar-scanner"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Le processus est terminé.'
        }
        success {
            echo 'Le build a réussi sans erreurs !'
        }
        failure {
            echo 'Le build a échoué. Veuillez vérifier les erreurs.'
        }
    }
}
