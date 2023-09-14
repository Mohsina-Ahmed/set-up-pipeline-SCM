pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = '1ce1d8fd-2d11-44c0-8cab-cda34015864a'
        AWS_CREDENTIALS = 'S3-Bucket'
        BUCKET_NAME = 'mohsina-bucket'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', credentialsId: env.GIT_CREDENTIALS, url: 'https://github.com/Mohsina-Ahmed/set-up-pipeline-SCM'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
                    . ~/.nvm/nvm.sh
                    nvm install 16
                    nvm use 16
                    npm install
                '''
            }
        }

        stage('Run HTML Check') {
            steps {
                sh '''
                    . ~/.nvm/nvm.sh
                    npm run check
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: env.AWS_CREDENTIALS, secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh 'aws s3 sync . s3://${BUCKET_NAME}/ --exclude "*" --include "*.html"'
                }
            }
        }
    }
}