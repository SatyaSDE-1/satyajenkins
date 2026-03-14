pipeline {
    agent any

    environment {
        APP_DIR = "/var/www/nest-app"
        APP_NAME = "nest-app"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                echo "Cleaning workspace..."
                cleanWs()
            }
        }

        stage('Clone Repository') {
            steps {
                echo "Cloning GitHub repository from main branch..."
                git branch: 'main', url: 'https://github.com/SatyaSDE-1/satyajenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh 'npm install'
            }
        }

        stage('Build Application') {
            steps {
                echo "Building NestJS application..."
                sh 'npm run build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo "Deploying application..."

                sh '''
                sudo mkdir -p $APP_DIR
                sudo rm -rf $APP_DIR/*
                sudo cp -r * $APP_DIR
                '''
            }
        }

        stage('Start Application') {
            steps {
                echo "Starting application using PM2..."

                sh '''
                cd $APP_DIR
                npm install --production

                pm2 delete $APP_NAME || true
                pm2 start dist/main.js --name $APP_NAME
                pm2 save
                '''
            }
        }
    }

    post {
        success {
            echo "🚀 NestJS deployment from MAIN branch completed successfully!"
        }

        failure {
            echo "❌ Deployment failed! Check Jenkins logs."
        }
    }
}