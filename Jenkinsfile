pipeline {
    agent any
  /*  {
        docker {
            image 'node:lts-bullseye-slim' 
            args '-p 3000:3000' 
        }
    } */
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') { 
            steps {
                sh 'npm test' 
            }
        }
    }
}