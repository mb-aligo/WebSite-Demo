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
        stage ('Zap Scan (SSH)') {
            steps{
                sshagent(credentials : ['ssh-key-thexoc11']) {                    
                    sh "ssh -o StrictHostKeyChecking=no root@thexoc11.aligo.corp 'ls -lrt'"
                    sh "ssh -o StrictHostKeyChecking=no root@thexoc11.aligo.corp 'docker exec -ti 66f78997a175 zap.sh -cmd -quickurl http://192.168.13.113:8083/ -quickout /home/zap/test-results-latest.html -quickprogress'"
                    sh 'ls -lrt'
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
