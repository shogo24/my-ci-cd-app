pipeline {
    agent {
        docker { 
            image 'node:20' // Use Node.js 20 image
            args '-u root:root' // optional, run as root to avoid permission issues
        }
    }

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        SITE_ID = 'c27607a5-a2f3-4842-b16f-30f8c4d3da02'
    }

    stages {
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