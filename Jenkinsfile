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
        stage('SonarQube Analysis') { 
            steps {
                scannerHome(tool: 'mySonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Build') { 
            steps {
                sh 'npm install' 
                sh 'npm run  build --if-present' 
            }
        }
        stage('Test') { 
            steps {
                sh 'npm test' 
            }
        }
    }
}