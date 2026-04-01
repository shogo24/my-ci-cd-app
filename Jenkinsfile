pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token') // Use the token you’ll add in Jenkins
        SITE_ID = 'c27607a5-a2f3-4842-b16c-30f8c4d3da02' // Your Netlify Site ID
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