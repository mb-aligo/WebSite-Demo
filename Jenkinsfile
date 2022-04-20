pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning Repository'
                sh 'rm -fr WebSite-Demo'
                sh 'git clone https://github.com/mb-aligo/WebSite-Demo.git'
            }
        }
        stage('Stage 2') {
            steps {
                echo 'Stage 2'
            }
        stage('Stage 3') {
            steps {
                echo 'Stage 3'
            }    
        }
    }
}
