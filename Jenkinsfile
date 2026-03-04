pipeline {
    agent any
    
    stages {

        stage('Checkout') {
            steps {
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'venkygit', url: 'https://github.com/veeravenkateswararao/react.git']])
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

         stage('Zip Artifact') {
            steps {
                sh 'zip -r react-build.zip build/'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'react-build.zip', fingerprint: true
            }
        }
        stage('Docker Build') {
             steps {
                 sh 'docker build -t react-app:${BUILD_NUMBER} .'
             }
         }
        stage('Deploy') {
             steps {
                sh '''
                    docker stop react-container || true
                    docker rm react-container || true
                    docker run -d -p 9676:80 --name react-container react-app:${BUILD_NUMBER}
                    '''
                    }
            }        
    }
}