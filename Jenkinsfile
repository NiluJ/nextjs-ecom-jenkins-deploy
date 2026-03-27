pipeline {
    agent any

    environment {
        APP_DIR = "/var/www/ecom-nextjs-app"

        NEXT_PUBLIC_SANITY_TOKEN = credentials('NEXT_PUBLIC_SANITY_TOKEN')
        NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY = credentials('NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY')
        NEXT_PUBLIC_STRIPE_SECRET_KEY = credentials('NEXT_PUBLIC_STRIPE_SECRET_KEY')
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/NiluJ/nextjs-ecom-jenkins-deploy.git'
            }
        }

        stage('Prepare Deployment Directory') {
            steps {
                sh '''
                mkdir -p $APP_DIR
                '''
            }
        }

        stage('Copy Files To Deployment Directory') {
            steps {
                sh '''
                cp -r * $APP_DIR
                '''
            }
        }

        stage('Install Dependencies (Deployment Dir)') {
            steps {
                sh '''
                cd $APP_DIR
                npm install --legacy-peer-deps
                '''
            }
        }

        stage('Build Application (Deployment Dir)') {
            steps {
                sh '''
                cd $APP_DIR
                npm run build
                '''
            }
        }

        stage('Stop Previous Application If Running') {
            steps {
                sh '''
                pkill -f "node" || true
                '''
            }
        }

        stage('Start Application Publicly') {
            steps {
                sh '''
                cd $APP_DIR
                nohup npm run start -- -H 0.0.0.0 -p 3000 > app.log 2>&1 &
                '''
            }
        }

        stage('Verify Application Running') {
            steps {
                sh '''
                sleep 5
                ss -tulnp | grep 3000 || exit 1
                '''
            }
        }
    }
}