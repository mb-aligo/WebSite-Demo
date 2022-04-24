pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-token')
    }
    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning Repository'
                sh 'rm -fr WebSite-Demo'
                sh 'git clone https://github.com/mb-aligo/WebSite-Demo.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building image from Dockerfile'
                sh 'docker build -t html-website-demo:latest .'
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }    
        }
        stage('Push') {
            steps {
                sh 'docker push html-website-demo:latest'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
