pipeline {
    agent any

    environment {
        APP_DIR = "/var/www/ecom-nextjs-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/NiluJ/nextjs-ecom-jenkins-deploy.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                mkdir -p $APP_DIR
                cp -r * $APP_DIR
                '''
            }
        }

        stage('Start Application') {
            steps {
                sh '''
                cd $APP_DIR
                nohup npm run start &
                '''
            }
        }
    }
}