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
                    // Clean up unnecessary Docker assets
                    sh 'docker system prune --force'

                    // Remove any existing container named "monapp" (if it exists)
                    sh 'docker rm -f monapp || true'

                    // Run the newly built container
                    sh 'docker run -d --name monapp --hostname monapp -p 8099:80 myimage_nginx'

                    // Run a command in the container without requiring a TTY
                    sh 'docker exec monapp ifconfig'
                }
            }
        }
    }
}
