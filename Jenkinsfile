pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')  // Your Jenkins credential ID
        SITE_ID = 'c27607a5-a2f3-4842-b16c-30f8c4d3da02'  // Your Netlify Site ID
    }

    stages {
        stage('Install Node') {
            steps {
                // Install curl and Node 20 inside the Jenkins container
                sh '''
                apt-get update -y
                apt-get install -y curl
                curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
                apt-get install -y nodejs
                node -v
                npm -v
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test -- --watchAll=false'
            }
        }

        stage('Deploy to Netlify') {
            steps {
                sh '''
                npm install -g netlify-cli
                netlify deploy --dir=build --prod --auth=$NETLIFY_AUTH_TOKEN --site=$SITE_ID
                '''
            }
        }
    }
}