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

        stage ('parallel test') {
            parallel {
                stage('1') {
                    steps {
                        echo "this is 1st"
                    }
                }

                stage('2') {
                    steps {
                        echo "this is 2nd"
                    }
                }
            }
        }

        stage('SonarCloud') {
            environment {
                SCANNER_HOME = tool 'mySonarScanner'
                ORGANIZATION = "utkarsh24695"
                PROJECT_NAME = "node-js-getting-started"
                }
            steps {
                withSonarQubeEnv('mySonar') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.organization=$ORGANIZATION \
                    -Dsonar.projectKey=$PROJECT_NAME \
                    -Dsonar.sources=.'''
                    }
                }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
                }
            }   
        }

        stage('Snyk Test') {
            steps {
                echo 'Testing...'
                // snykSecurity(
                // snykInstallation: 'mysnyk',
                // snykTokenId: 'mysnyktoken',
                // place other parameters here
                // )
                snykSecurity organisation: 'utkarsh.sharma-t1s', projectName: 'node-js-getting-started', severity: 'medium', snykInstallation: 'mysnyk', snykTokenId: 'mysnyktoken', targetFile: 'package.json'
                //sh "snyk test  --json --severity-threshold=low --all-projects"
                //sh "snyk code test"
            }
        }

        stage('Build') { 
            steps {
                sh 'npm install' 
            //    sh 'npm run  build --if-present' 
            }
        }
        
        stage('Test') { 
            steps {
            //    sh 'npm test' 
            echo 'Testing skip...'
            }
        }

        stage('Artifact to S3') {
            steps {
                // moveComponents destination: 'mynode', nexusInstanceId: 'nexus3', tagName: 'build1'
                sh 'zip -r ot-facctum.zip .'
                s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'facctums3', excludedFile: '', flatten: false, gzipFiles: true, keepForever: false, managedArtifacts: true, noUploadOnFailure: false, selectedRegion: 'ap-south-1', showDirectlyInBrowser: false, sourceFile: '**/ot-facctum.zip', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 's3upload', userMetadata: []            
                }
        }
    }
    triggers {
    pollSCM('')
  }
}