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
        stage('Build (Dockerile)') {
            steps {
                echo 'Building image from Dockerfile'
                sh 'docker build -t mbaligo/mb-aligo-website-demo:latest .'
            }
        }
        stage('Login/Publish (DockerHub)') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
		sh 'docker push mbaligo/mb-aligo-website-demo:latest'
            }    
        }
        stage ('Sonar Scan (On Host)') {
            steps{
                sh 'sonar-scanner   -Dsonar.projectKey=Aligo-Demo   -Dsonar.sources=.   -Dsonar.host.url=http://192.168.13.113:8082   -Dsonar.login=918025ba242d5cfc28fffcfcbdca71f9eabc5949'
            }
        }
        stage ('Zap Scan (SSH)') {
            steps{
                sshagent(credentials : ['ssh-key-thexoc11']) {               
                    sh "ssh -o StrictHostKeyChecking=no root@thexoc11.aligo.corp 'docker exec 5035a0148a3d zap.sh -cmd -quickurl http://192.168.13.133:8083/ -quickout /home/zap/test-results-latest.html -quickprogress'"
                }
		sshagent(credentials : ['ssh-key-ctlpln01']) {                    
                    sh "ssh -o StrictHostKeyChecking=no root@ctlpln01.aligo.corp 'kubectl rollout restart deploy web-demo -n default'"
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
