pipeline {
    agent any
  /*  {
        docker {
            image 'node:lts-bullseye-slim' 
            args '-p 3000:3000' 
        }
    } */
    stages {
        stage('checkout'){
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitlogin', url: 'https://github.com/utkarsh24695/node-js-getting-started.git']]])
            }
        }
        stage('Test') { 
            steps {
                sh 'npm install' 
                sh 'npm test' 
            }
        }
        stage('Build') { 
            steps {
                sh 'npm run  build --if-present' 
            }
        }
    }
}