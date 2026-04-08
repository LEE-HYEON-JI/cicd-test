pipeline {
    agent any
    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/LEE-HYEON-JI/cicd-test.git'
            }
        }
        stage('Git clone end') {
            steps {
                sh 'touch cicd_test.txt'
                sh 'echo "git clone end" > cicd_test.txt'
            }
        }
        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh 'sudo scp -o StrictHostKeyChecking=no index.html unbuntu@3.36.95.189:/var/www/html/'
                }
            }
        }
    }
}
