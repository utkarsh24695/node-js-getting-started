pipeline {
    agent any
  /*  {
        docker {
            image 'node:lts-bullseye-slim' 
            args '-p 3000:3000' 
        }
    } */
    stages {
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