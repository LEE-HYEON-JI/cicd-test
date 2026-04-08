import java.text.SimpleDateFormat

def TODAY = (new SimpleDateFormat("yyyyMMdd")).format(new Date())

pipeline {
    agent any

    environment {
        strDockerTag="${TODAY}_${BUILD_ID}"
        strDockerImage = "01hyeonji/cicd-test:${strDockerTag}"
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
        stage('Docker Image Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-auth') {
                        oDockImage.push()
                    }
                }
            }
        }
        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.36.95.189 docker container rm -f sampleweb'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.36.95.189 docker run -d -p 80:80 --name sampleweb ${strDockerImage}'
                }
            }
        }
    }
}
