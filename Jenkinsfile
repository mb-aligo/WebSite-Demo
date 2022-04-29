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
                sh 'docker build -t mbaligo/mb-aligo-website-demo:latest .'
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }    
        }
        stage('Push') {
            steps {
                sh 'docker push mbaligo/mb-aligo-website-demo:latest'
            }
        }
        def remote = [:]
        remote.name = 'thexoc11test'
        remote.host = 'thexoc11'
        remote.user = 'root'
        remote.password = 'Lnx090909!'
        remote.allowAnyHosts = true
        stage('Remote SSH') {
            sshCommand remote: remote, command: "ls -lrt"
        }       
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
