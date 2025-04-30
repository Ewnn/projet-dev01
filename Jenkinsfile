pipeline {
    agent any
    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t myimage_nginx .'
                    sh 'docker tag myimage_nginx iot:myimage_nginx'
                }
            }
        }
        stage('Deployment App') {
            steps {
                script {
                    sh 'docker system prune --force'
                    sh 'docker rm -f monapp || true'
                    sh 'docker run -d --name monapp --hostname monapp -p 8099:80 myimage_nginx'
                    sh 'docker exec monapp ifconfig'
                }
            }
        }
    }
}
