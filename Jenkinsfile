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
        stage ('Deploy') {
            steps{
                sshagent(credentials : ['ssh-key-thexoc11']) {
                    sh 'ls -lrt'
		    sh 'docker ps'
		    sh 'docker exec -ti f52a683f9b53 zap.sh -cmd -quickurl http://192.168.13.113:8083/ -quickout /home/zap/test-results-%BUILD_NUMBER%.html -quickprogress'
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
