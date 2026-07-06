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
                sh 'pm2 restart multi-auth'
            }
        }

        stage('Health Check') {
            steps {
                sh 'curl http://localhost/auth'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }
    }
}
