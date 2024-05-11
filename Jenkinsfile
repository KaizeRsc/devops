pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                dir('devops/myproject') { // Chemin vers le dossier contenant le pom.xml
                    sh 'mvn clean package'
                }
            }
        }

        stage('Test') {
            steps {
                dir('devops/myproject') { // Chemin vers le dossier contenant le pom.xml
                    sh 'mvn test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('devops/myproject') { // Chemin vers le dossier contenant le pom.xml
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
