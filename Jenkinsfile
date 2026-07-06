pipeline {
    agent any

    environment {
        PATH = "/usr/bin:/usr/local/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Generate Prisma Client') {
            steps {
                sh 'npx prisma generate'
            }
        }

        stage('Restart PM2') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.84 "
                        cd /home/ubuntu/Multi-Auth &&
                        pm2 restart multi-auth
                    "
                    '''
                }
            }
        }

        stage('Health Check') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.84 "
                        curl http://localhost/auth
                    "
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            sshagent(['ec2-key']) {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.84 "
                    cd /home/ubuntu/Multi-Auth &&
                    git reset --hard HEAD~1 &&
                    npm install &&
                    npx prisma generate &&
                    pm2 restart multi-auth
                "
                '''
            }
            echo 'Deployment Failed - Rollback Completed'
        }
    }
}
