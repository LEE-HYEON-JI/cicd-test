pipeline {
    agent any

    environment {
        strDockerImage = "01hyeonji/cicd-test:latest"
    }
    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/LEE-HYEON-JI/cicd-test.git'
            }
        }
        stage('Docker Image Build') {
            steps {
                script {
                    oDockImage = docker.build(strDockerImage,"-f Dockerfile .")
                }
            }
        }
        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh 'scp -o StrictHostKeyChecking=no index.html ubuntu@3.36.95.189:/home/ubuntu/'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.36.95.189 sudo cp /home/ubuntu/index.html /var/www/html/'
                }
            }
        }
    }
}
