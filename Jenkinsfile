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
                dir('devops/myproject') { // Assurez-vous que ce chemin est correct
                    sh 'ls -la' // Ajout pour déboguer et voir les fichiers présents
                    sh 'mvn clean package'
                }
            }
        }

        stage('Test') {
            steps {
                dir('devops/myproject') {
                    sh 'mvn test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('devops/myproject') {
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
