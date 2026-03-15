pipeline {
    agent any

    environment {
        // Name of your SonarQube server configured in Jenkins
        SONARQUBE_SERVER = 'sonarqube'
        // SonarQube token credential ID
        SONAR_AUTH_TOKEN_ID = 'sonar-token-id'
        // DockerHub credentials
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds-id'
        IMAGE_NAME = 'prem07p/simple-cicd-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: env.SONAR_AUTH_TOKEN_ID, variable: 'SONAR_AUTH_TOKEN')]) {
                    withSonarQubeEnv(env.SONARQUBE_SERVER) {
                        script {
                            // Use installed SonarScanner tool
                            def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                            sh """${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=simple-cicd-app \
                                -Dsonar.projectName=simple-cicd-app \
                                -Dsonar.sources=. \
                                -Dsonar.login=$SONAR_AUTH_TOKEN"""
                        }
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: env.DOCKERHUB_CREDENTIALS, url: '']) {
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker stop simple-cicd-app || true
                docker rm simple-cicd-app || true
                docker run -d --name simple-cicd-app -p 5000:5000 ${IMAGE_NAME}
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
