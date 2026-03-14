// /Users/anurag/jenkins/Jenkinsfile
node {

    def appDir = "/var/www/nest-app"

    stage('Clean Workspace') {
        echo "Cleaning workspace..."
        cleanWs()
    }

    stage('Clone Repository') {
        echo "Cloning GitHub repository from main branch..."

        git branch: 'main', url: 'https://github.com/SatyaSDE-1/satyajenkins.git'
    }

    stage('Install Dependencies') {
        echo "Installing dependencies..."
        sh 'npm install'
    }

    stage('Build Application') {
        echo "Building NestJS application..."
        sh 'npm run build'
    }

    stage('Deploy Application') {
        echo "Deploying application to server..."

        sh """
        sudo mkdir -p ${appDir}
        sudo rm -rf ${appDir}/*
        sudo cp -r * ${appDir}
        """
    }

    stage('Start Application') {
        echo "Starting application using PM2..."

        sh """
        cd ${appDir}
        npm install
        pm2 delete nest-app || true
        pm2 start dist/main.js --name nest-app
        """
    }

    stage('Deployment Complete') {
        echo "🚀 NestJS deployment from MAIN branch completed successfully!"
    }
}