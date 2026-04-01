pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        SITE_ID = 'c27607a5-a2f3-4842-b16c-30f8c4d3da02'
    }

    stages {
        stage('Install Node') {
            steps {
                sh '''
                apt-get update && apt-get install -y curl
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