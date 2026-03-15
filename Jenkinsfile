pipeline {
    agent any

    tools {
        sonarQube 'sonar-scanner'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/YOUR_GITHUB_USERNAME/simple-cicd-app.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t YOUR_DOCKERHUB_USERNAME/simple-cicd-app .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker push YOUR_DOCKERHUB_USERNAME/simple-cicd-app'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 5000:5000 YOUR_DOCKERHUB_USERNAME/simple-cicd-app'
            }
        }
    }
}
