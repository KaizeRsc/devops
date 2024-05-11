pipeline {
    agent any

    tools {
        // Assurez-vous que 'Maven' est configuré dans Jenkins sous Global Tool Configuration
        maven 'Maven'
    }

    environment {
        // Ajoutez Maven au PATH pour s'assurer qu'il est accessible dans les étapes du script
        PATH = "${tool 'Maven'}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupération du code source
                checkout scm
                // Commande pour déboguer et afficher le contenu du répertoire actuel après le checkout
                sh 'ls -la'
                // Débogue pour afficher le contenu du sous-dossier prévu pour le Maven 'pom.xml'
                dir('devops/myproject') {
                    sh 'ls -la'
                }
            }
        }

        stage('Build') {
            steps {
                // Naviguer dans le répertoire contenant le pom.xml
                dir('devops/myproject') {
                    // Vérifier la version de Maven pour s'assurer que Maven est accessible
                    sh 'mvn --version'
                    // Exécuter Maven pour nettoyer et construire le projet
                    sh 'mvn clean package'
                }
            }
        }

        stage('Test') {
            steps {
                dir('devops/myproject') {
                    // Exécuter les tests Maven
                    sh 'mvn test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('devops/myproject') {
                    // Effectuer l'analyse SonarQube
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar'
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
