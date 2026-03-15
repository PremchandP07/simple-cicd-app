pipeline {
    agent any

    environment {
        // Optional: define your SonarQube server name (from Jenkins config)
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Inject the token from Jenkins Credentials
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_AUTH_TOKEN')]) {
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh """
                        sonar-scanner \
                            -Dsonar.projectKey=simple-cicd-app \
                            -Dsonar.projectName=simple-cicd-app \
                            -Dsonar.sources=. \
                            -Dsonar.login=$SONAR_AUTH_TOKEN
                        """
                    }
                }
            }
        }
    }
}
